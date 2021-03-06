---
title: Självstudie – etablera X. 509-enheter till Azure IoT Hub med en anpassad HSM-modul (Hardware Security Module)
description: I den här självstudien används registrerings grupper. I den här självstudien får du lära dig hur du etablerar X. 509-enheter med hjälp av en anpassad HSM (Hardware Security Module) och C-enhets-SDK för Azure IoT Hub Device Provisioning Service (DPS).
author: wesmc7777
ms.author: wesmc
ms.date: 11/18/2020
ms.topic: tutorial
ms.service: iot-dps
services: iot-dps
ms.custom: mvc
ms.openlocfilehash: f6026680dd566bf7a13c83b37883341bff8b4570
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/30/2020
ms.locfileid: "96355182"
---
# <a name="tutorial-provision-multiple-x509-devices-using-enrollment-groups"></a>Självstudie: etablera flera X. 509-enheter med hjälp av registrerings grupper

I den här självstudien får du lära dig hur du etablerar grupper av IoT-enheter som använder X. 509-certifikat för autentisering. Exempel kod från [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) används för att etablera din utvecklings maskin som en IoT-enhet. 

Azure IoT Device Provisioning Service stöder två typer av registreringar:

* [Registreringsgrupper](concepts-service.md#enrollment-group): används för att registrera flera relaterade enheter.
* [Enskilda registreringar](concepts-service.md#individual-enrollment): används för att registrera en enskild enhet.

Den här självstudien liknar de tidigare självstudierna som demonstrerar hur du använder registrerings grupper för att etablera uppsättningar med enheter. Men X. 509-certifikat kommer att användas i den här självstudien i stället för symmetriska nycklar. Granska de föregående självstudierna i det här avsnittet för en enkel metod med hjälp av [symmetriska nycklar](./concepts-symmetric-key-attestation.md).

I den här självstudien visas det [anpassade HSM-exemplet](https://github.com/Azure/azure-iot-sdk-c/tree/master/provisioning_client/samples/custom_hsm_example) som tillhandahåller en stub-implementering för samverkan med maskinvarubaserad säker lagring. En [modul för maskin varu säkerhet (HSM)](./concepts-service.md#hardware-security-module) används för säker, maskinvarubaserad lagring av enhets hemligheter. En HSM kan användas med symmetrisk nyckel, X. 509-certifikat eller TPM-attestering för att tillhandahålla säker lagring för hemligheter. Maskin vara-baserad lagring av enhets hemligheter rekommenderas starkt.

Om du inte är bekant med processen för autoetablering, granskar du [etablerings](about-iot-dps.md#provisioning-process) översikten. Kontrol lera också att du har slutfört stegen i [konfigurera IoT Hub Device Provisioning service med Azure Portal](quick-setup-auto-provision.md) innan du fortsätter med den här självstudien. 


I den här självstudien ska du slutföra följande mål:

> [!div class="checklist"]
> * Skapa en certifikat kedja med förtroende för att organisera en uppsättning enheter med X. 509-certifikat.
> * Fullständigt bevis på innehav med ett signerings certifikat som används med certifikat kedjan.
> * Skapa en ny grupp registrering som använder certifikat kedjan
> * Konfigurera utvecklings miljön för etablering av en enhet med hjälp av kod från [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c)
> * Etablera en enhet med hjälp av certifikat kedjan med hjälp av HSM-exemplet (Custom Hardware Security Module) i SDK: n.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Förutsättningar

Följande förutsättningar gäller för en Windows-utvecklings miljö. För Linux eller macOS, se lämpligt avsnitt i [förbereda utvecklings miljön](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) i SDK-dokumentationen.

* [Visual Studio](https://visualstudio.microsoft.com/vs/) 2019 med arbets belastningen ["Skriv bords utveckling med C++"](https://docs.microsoft.com/cpp/ide/using-the-visual-studio-ide-for-cpp-desktop-development) aktiverat. Visual Studio 2015 och Visual Studio 2017 stöds också.

* Senaste versionen av [Git](https://git-scm.com/download/) installerad.

## <a name="prepare-the-azure-iot-c-sdk-development-environment"></a>Förbered utvecklings miljön för Azure IoT C SDK

I det här avsnittet förbereder du en utvecklingsmiljö som används för att skapa [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c). SDK innehåller exempel kod och verktyg som används av X. 509-enheters etablering med DPS.

1. Ladda ned [cmake build-systemet](https://cmake.org/download/).

    Det är viktigt att Visual Studio-förutsättningarna ([Visual Studio](https://visualstudio.microsoft.com/vs/) och arbets belastningen [Skriv bords utveckling med C++](https://docs.microsoft.com/cpp/ide/using-the-visual-studio-ide-for-cpp-desktop-development) ) är installerade på datorn **innan** du påbörjar `CMake` installationen. När förutsättningarna är uppfyllda och nedladdningen har verifierats installerar du CMake-byggesystemet.

2. Hitta taggnamnet för den [senaste versionen](https://github.com/Azure/azure-iot-sdk-c/releases/latest) av Azure IoT C SDK.

3. Öppna en kommandotolk eller Git Bash-gränssnittet. Kör följande kommandon för att klona den senaste versionen av [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) GitHub-lagringsplatsen. Använd taggen som du hittade i föregående steg som `-b` parameter värde:

    ```cmd/sh
    git clone -b <release-tag> https://github.com/Azure/azure-iot-sdk-c.git
    cd azure-iot-sdk-c
    git submodule update --init
    ```

    Den här åtgärden kan förväntas ta flera minuter att slutföra.

4. Skapa en `cmake`-underkatalog i rotkatalogen på git-lagringsplatsen och navigera till den mappen. 

    ```cmd/sh
    mkdir cmake
    cd cmake
    ```

5. `cmake`Katalogen som du skapade innehåller det anpassade HSM-exemplet och den exempel enhets etablerings kod som använder anpassad HSM för att tillhandahålla X. 509-autentisering. 

    Kör följande kommando i din `cmake` katalog för att skapa en version av SDK som är specifika för din utvecklings plattform. Versionen innehåller en referens till det anpassade HSM-exemplet. 

    När du anger sökvägen som används med `-Dhsm_custom_lib` nedan, se till att du använder sökvägen i förhållande till den `cmake` katalog som du skapade tidigare. Den relativa sökvägen som visas nedan är bara ett exempel.

    ```cmd
    $ cmake -Duse_prov_client:BOOL=ON -Dhsm_custom_lib=/d/azure-iot-sdk-c/cmake/provisioning_client/samples/custom_hsm_example/Debug/custom_hsm_example.lib ..
    ```

    Om `cmake` inte hittar din C++-kompilerare kan du få kompileringsfel när du kör kommandot ovan. Om det händer ska du försöka köra det här kommandot i [kommandotolken i Visual Studio](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs).

    När bygget lyckas genereras en Visual Studio-lösning i din `cmake` katalog. De senaste utmatnings raderna ser ut ungefär så här:

    ```cmd/sh
    $ cmake -Duse_prov_client:BOOL=ON -Dhsm_custom_lib=/d/azure-iot-sdk-c/cmake/provisioning_client/samples/custom_hsm_example/Debug/custom_hsm_example.lib ..
    -- Building for: Visual Studio 16 2019
    -- The C compiler identification is MSVC 19.23.28107.0
    -- The CXX compiler identification is MSVC 19.23.28107.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: D:/azure-iot-sdk-c/cmake
    ```

## <a name="create-an-x509-certificate-chain"></a>Skapa en X. 509-certifikat kedja

I det här avsnittet ska du skapa en X. 509-kedja av tre certifikat för testning med den här självstudien. Certifikaten kommer att ha följande hierarki.

![Självstudie för enhets certifikat kedja](./media/tutorial-custom-hsm-enrollment-group-x509/example-device-cert-chain.png#lightbox)

[Rot certifikat](concepts-x509-attestation.md#root-certificate): du kommer att fylla i [bevis på innehav](how-to-verify-certificates.md) för att verifiera rot certifikatet. Den här kontrollen gör att DPS litar på certifikatet och verifierar certifikat som signerats av det.

[Mellanliggande certifikat](concepts-x509-attestation.md#intermediate-certificate): det är vanligt att använda mellanliggande certifikat för att gruppera enheter logiskt av produkt linjer, företags divisioner eller andra kriterier. I den här självstudien används en certifikat kedja bestående av ett mellanliggande certifikat. Det mellanliggande certifikatet kommer att signeras av rot certifikatet. Det här certifikatet används också på registrerings gruppen som skapas i DPS för att logiskt gruppera en uppsättning enheter. Med den här konfigurationen kan du hantera en hel grupp av enheter som har enhets certifikat signerade av samma mellanliggande certifikat. Du kan skapa registrerings grupper för att aktivera eller inaktivera en grupp av enheter. Mer information om hur du inaktiverar en grupp av enheter finns i [Tillåt inte en mellanliggande X. 509-eller rotcertifikatutfärdarcertifikat med hjälp av en registrerings grupp](how-to-revoke-device-access-portal.md#disallow-an-x509-intermediate-or-root-ca-certificate-by-using-an-enrollment-group)

[Enhets certifikat](concepts-x509-attestation.md#end-entity-leaf-certificate): enhets certifikatet (löv) kommer att signeras av det mellanliggande certifikatet och lagras på enheten tillsammans med dess privata nyckel. Enheten kommer att presentera det här certifikatet och den privata nyckeln, tillsammans med certifikat kedjan vid försök att tillhandahålla etableringen. 

Så här skapar du certifikat kedjan:

1. Öppna en git-bash kommando tolk. Slutför steg 1 och 2 med hjälp av bash shell-instruktionerna som finns i [Hantera test CA-certifikat för exempel och självstudier](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md#managing-test-ca-certificates-for-samples-and-tutorials).

    Det här steget skapar en arbets katalog för certifikat skripten och genererar exempel roten och mellanliggande certifikat för certifikat kedjan med OpenSSL. 

    Observera i utdata som visar platsen för det självsignerade rot certifikatet. Det här certifikatet genomgår [bevis på innehav](how-to-verify-certificates.md) för att verifiera ägarskapet senare.

    ```output
    Creating the Root CA Certificate
    CA Root Certificate Generated At:
    ---------------------------------
        ./certs/azure-iot-test-only.root.ca.cert.pem
    
    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number:
                fc:cc:6b:ab:3b:9a:3e:fe
        Signature Algorithm: sha256WithRSAEncryption
            Issuer: CN=Azure IoT Hub CA Cert Test Only
            Validity
                Not Before: Oct 23 21:30:30 2020 GMT
                Not After : Nov 22 21:30:30 2020 GMT
            Subject: CN=Azure IoT Hub CA Cert Test Only
    ```        

    Observera i utdata som visar platsen för det mellanliggande certifikatet som signeras/utfärdas av rot certifikatet. Certifikatet kommer att användas med registrerings gruppen som du skapar senare.

    ```output
    Intermediate CA Certificate Generated At:
    -----------------------------------------
        ./certs/azure-iot-test-only.intermediate.cert.pem
    
    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number: 1 (0x1)
        Signature Algorithm: sha256WithRSAEncryption
            Issuer: CN=Azure IoT Hub CA Cert Test Only
            Validity
                Not Before: Oct 23 21:30:33 2020 GMT
                Not After : Nov 22 21:30:33 2020 GMT
            Subject: CN=Azure IoT Hub Intermediate Cert Test Only
    ```    

2. Kör sedan följande kommando för att skapa ett nytt enhets-eller löv certifikat med ett ämnes namn som du anger som parameter. Använd det exempel på ämnes namnet som angavs för den här självstudien `custom-hsm-device-01` . Det här ämnes namnet är enhets-ID: t för din IoT-enhet. 

    > [!WARNING]
    > Använd inte ett ämnes namn med blank steg. Det här ämnes namnet är enhets-ID: t för den IoT-enhet som tillhandahålls. Det måste följa reglerna för ett enhets-ID. Mer information finns i [Egenskaper för enhets identitet](../iot-hub/iot-hub-devguide-identity-registry.md#device-identity-properties).

    ```cmd
    ./certGen.sh create_device_certificate_from_intermediate "custom-hsm-device-01"
    ```

    Observera följande utdata som visar var det nya enhets certifikatet finns. Enhetens certifikat är signerat (utfärdat) av mellanliggande certifikat.

    ```output
    -----------------------------------
    ./certs/new-device.cert.pem: OK
    Leaf Device Certificate Generated At:
    ----------------------------------------
        ./certs/new-device.cert.pem
    
    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number: 9 (0x9)
        Signature Algorithm: sha256WithRSAEncryption
            Issuer: CN=Azure IoT Hub Intermediate Cert Test Only
            Validity
                Not Before: Nov 10 09:20:33 2020 GMT
                Not After : Dec 10 09:20:33 2020 GMT
            Subject: CN=custom-hsm-device-01
    ```    
    
3. Kör följande kommando för att skapa en fullständig certifikat kedja. PEM-fil som innehåller det nya enhets certifikatet.

    ```Bash
    cd ./certs && cat new-device.cert.pem azure-iot-test-only.intermediate.cert.pem azure-iot-test-only.root.ca.cert.pem > new-device-full-chain.cert.pem
    ```

    Använd en text redigerare och öppna filen med certifikat kedjan, *./certs/New-Device-full-Chain.cert.pem*. Texten i certifikat kedjan innehåller en fullständig kedja av alla tre certifikaten. Du kommer att använda den här texten som certifikat kedja med den anpassade HSM-koden längre fram i den här självstudien.

    Den fullständiga kedje texten har följande format:
 
    ```output 
    -----BEGIN CERTIFICATE-----
        <Text for the device certificate includes public key>
    -----END CERTIFICATE-----
    -----BEGIN CERTIFICATE-----
        <Text for the intermediate certificate includes public key>
    -----END CERTIFICATE-----
    -----BEGIN CERTIFICATE-----
        <Text for the root certificate includes public key>
    -----END CERTIFICATE-----
    ```

5. Observera att den privata nyckeln för det nya enhets certifikatet skrivs till *./Private/New-Device.Key.pem*. Texten för den här nyckeln kommer att behövas av enheten under etableringen. Texten kommer att läggas till i det anpassade HSM-exemplet senare.

    > [!WARNING]
    > Texten för certifikaten innehåller endast information om offentliga nycklar. 
    >
    > Enheten måste dock också ha åtkomst till den privata nyckeln för enhets certifikatet. Detta är nödvändigt eftersom enheten måste utföra verifieringen med nyckeln vid körning vid försök att tillhandahålla etableringen. Den här nyckelns känslighet är en av de viktigaste orsakerna till att du bör använda maskinvarubaserad lagring i en riktig HSM för att skydda privata nycklar.



## <a name="configure-the-custom-hsm-stub-code"></a>Konfigurera anpassad HSM stub-kod

De specifika värdena för att interagera med faktisk säker maskinvarubaserad lagring varierar beroende på maskin varan. Därför kommer den certifikat kedja som används av enheten i den här självstudien att hårdkodad i den anpassade HSM-stub-koden. I ett verkligt scenario lagras certifikat kedjan i den faktiska HSM-maskinvaran för att ge bättre säkerhet för känslig information. Metoder som liknar de stub-metoder som visas i det här exemplet implementeras sedan för att läsa hemligheter från den maskinbaserade lagringen.

Så här uppdaterar du den anpassade HSM stub-koden för den här självstudien:

1. Starta Visual Studio och öppna den nya lösnings filen som skapades i den `cmake` katalog som du skapade i roten för Azure-IoT-SDK-c git-lagringsplatsen. Lösnings filen heter `azure_iot_sdks.sln` .

2. I Solution Explorer för Visual Studio navigerar du till **Provisioning_Samples > custom_hsm_example > källfiler** och öppnar *custom_hsm_example. c*.

3. Uppdatera strängvärdet för `COMMON_NAME` strängkonstant med det nätverks namn som du använde när du genererade enhets certifikatet.

    ```c
    static const char* const COMMON_NAME = "custom-hsm-device-01";
    ```

4. I samma fil uppdaterar du strängvärdet för en `CERTIFICATE` konstant sträng med hjälp av din certifikat kedje text som du sparade i *./certs/New-Device-full-Chain.cert.pem* när du har genererat dina certifikat.

    > [!IMPORTANT]
    > När du kopierar texten till Visual Studio kan du märka att texten tolkas och uppdateras med kod avstånd osv. I så fall måste du ta bort det här avståndet och parsningen genom att trycka på **CTRL + Z** en gång.

    Uppdatera certifikat texten så att den följer mönstret nedan utan extra mellanslag eller parsning som gjorts av Visual Studio:

    ```c
    // <Device/leaf cert>
    // <intermediates>
    // <root>
    static const char* const CERTIFICATE = "-----BEGIN CERTIFICATE-----\n"
    "MIIFOjCCAyKgAwIBAgIJAPzMa6s7mj7+MA0GCSqGSIb3DQEBCwUAMCoxKDAmBgNV"
        ...
    "MDMwWhcNMjAxMTIyMjEzMDMwWjAqMSgwJgYDVQQDDB9BenVyZSBJb1QgSHViIENB"
    "\n-----END CERTIFICATE-----\n"
    "-----BEGIN CERTIFICATE-----\n"
    "MIIFPDCCAySgAwIBAgIBATANBgkqhkiG9w0BAQsFADAqMSgwJgYDVQQDDB9BenVy"
        ...
    "MTEyMjIxMzAzM1owNDEyMDAGA1UEAwwpQXp1cmUgSW9UIEh1YiBJbnRlcm1lZGlh"
    "\n-----END CERTIFICATE-----\n"
    "-----BEGIN CERTIFICATE-----\n"
    "MIIFOjCCAyKgAwIBAgIJAPzMa6s7mj7+MA0GCSqGSIb3DQEBCwUAMCoxKDAmBgNV"
        ...
    "MDMwWhcNMjAxMTIyMjEzMDMwWjAqMSgwJgYDVQQDDB9BenVyZSBJb1QgSHViIENB"
    "\n-----END CERTIFICATE-----";        
    ```

5. I samma fil uppdaterar du strängvärdet för `PRIVATE_KEY` konstant sträng med hjälp av den privata nyckeln för ditt enhets certifikat.

    > [!IMPORTANT]
    > När du kopierar texten till Visual Studio kan du märka att texten tolkas och uppdateras med kod avstånd osv. I så fall måste du ta bort det här avståndet och parsningen genom att trycka på **CTRL + Z** en gång.

    Uppdatera texten för den privata nyckeln så att den följer mönstret nedan utan extra mellanslag eller parsning som gjorts av Visual Studio:

    ```c
    static const char* const PRIVATE_KEY = "-----BEGIN RSA PRIVATE KEY-----\n"
    "MIIJJwIBAAKCAgEAtjvKQjIhp0EE1PoADL1rfF/W6v4vlAzOSifKSQsaPeebqg8U"
        ...
    "X7fi9OZ26QpnkS5QjjPTYI/wwn0J9YAwNfKSlNeXTJDfJ+KpjXBcvaLxeBQbQhij"
    "\n-----END RSA PRIVATE KEY-----";
    ```

6. Spara *custom_hsm_example. c*.


## <a name="verify-ownership-of-the-root-certificate"></a>Verifiera ägarskap för rot certifikatet

1. Använd anvisningarna från [registrera den offentliga delen av ett X. 509-certifikat och få en verifierings kod](how-to-verify-certificates.md#register-the-public-part-of-an-x509-certificate-and-get-a-verification-code), ladda upp rot certifikatet och få en verifierings kod från DPS.

2. När du har en verifierings kod från DPS för rot certifikatet kör du följande kommando från certifikat skriptets arbets katalog för att generera ett verifierings certifikat.
 
    Verifierings koden som anges här är bara ett exempel. Använd koden som du skapade från DPS.    

    ```Bash
    ./certGen.sh create_verification_certificate 1B1F84DE79B9BD5F16D71E92709917C2A1CA19D5A156CB9F    
    ```    

    Det här skriptet skapar ett certifikat som signerats av rot certifikatet med ämnes namnet inställt på verifierings koden. Med det här certifikatet kan DPS verifiera att du har åtkomst till rot certifikatets privata nyckel. Lägg märke till platsen för verifierings certifikatet i utdata från skriptet.

    ```output
    Leaf Device PFX Certificate Generated At:
    --------------------------------------------
        ./certs/verification-code.cert.pfx
    ```

3. Ladda upp [det signerade verifierings certifikatet](how-to-verify-certificates.md#upload-the-signed-verification-certificate), ladda upp verifierings certifikatet och klicka på **Verifiera** i DPS för att slutföra bevis på innehavet för rot certifikatet.


## <a name="update-the-certificate-store-on-windows-based-devices"></a>Uppdatera certifikat arkivet på Windows-baserade enheter

På icke-Windows-enheter kan du skicka certifikat kedjan från koden som certifikat arkiv.

På Windows-baserade enheter måste du lägga till signerings certifikaten (rot och mellanliggande) till ett Windows- [certifikat Arkiv](https://docs.microsoft.com/windows/win32/secauthn/certificate-stores). Annars transporteras signerings certifikaten inte till DPS genom en säker kanal med Transport Layer Security (TLS).

Lägga till signerings certifikaten i certifikat arkivet i Windows-baserade enheter:

1. I en git bash-prompt navigerar du till den under `certs` katalog som innehåller dina signerings certifikat och konverterar dem till `.pfx` enligt följande.

    rot certifikat:

    ```bash
    winpty openssl pkcs12 -inkey ../private/azure-iot-test-only.root.ca.key.pem -in ./azure-iot-test-only.root.ca.cert.pem -export -out ./root.pfx
    ```
    
    mellanliggande certifikat:   

    ```bash
    winpty openssl pkcs12 -inkey ../private/azure-iot-test-only.intermediate.key.pem -in ./azure-iot-test-only.intermediate.cert.pem -export -out ./intermediate.pfx
    ```

2. Högerklicka på **Start** -knappen i Windows. Vänsterklicka på **Kör**. Ange *certmgr. MCS* och klicka på **OK** för att starta MMC-snapin-modulen för certifikat hanteraren.

3. I certifikat hanteraren, under **certifikat – aktuell användare**, klickar du på **betrodda rot certifikat utfärdare**. Gå sedan till menyn och klicka på **åtgärden**  >  **alla aktiviteter**  >  **Importera** som ska importeras `root.pfx` .

    * Se till att söka efter **personal information Exchange (. pfx)**
    * Använd `1234` som lösen ord.
    * Placera certifikatet i certifikat arkivet **betrodda rot certifikat utfärdare** .

4. I certifikat hanteraren, under **certifikat – aktuell användare**, klickar du på **mellanliggande certifikat utfärdare**. Gå sedan till menyn och klicka på **åtgärden**  >  **alla aktiviteter**  >  **Importera** som ska importeras `intermediate.pfx` .

    * Se till att söka efter **personal information Exchange (. pfx)**
    * Använd `1234` som lösen ord.
    * Placera certifikatet i certifikat arkivet **mellanliggande certifikat utfärdare** .

Dina signerings certifikat är nu betrodda på den Windows-baserade enheten och den fullständiga kedjan kan transporteras till DPS.



## <a name="create-an-enrollment-group"></a>Skapa en registrerings grupp

1. Logga in på Azure Portal, Välj knappen **alla resurser** i den vänstra menyn och öppna Device Provisioning-tjänsten.

2. Välj fliken **Hantera registreringar** och välj sedan knappen **Lägg till registrerings grupp** överst.

3. I panelen **Lägg till registrerings grupp** anger du följande information och trycker sedan på knappen **Spara** .

      ![Lägg till registrerings grupp för X. 509-attestering i portalen](./media/tutorial-custom-hsm-enrollment-group-x509/custom-hsm-enrollment-group-x509.png#lightbox)

    | Fält        | Värde           |
    | :----------- | :-------------- |
    | **Gruppnamn** | I den här självstudien anger du **Custom-HSM-x509-Devices** |
    | **Attesterings typ** | Välj **certifikat** |
    | **IoT Edge-enhet** | Välj **falskt** |
    | **Certifikat typ** | Välj **mellanliggande certifikat** |
    | **Primär Certificate. pem eller. cer-fil** | Navigera till den mellanliggande du skapade tidigare (*./certs/Azure-IoT-test-Only.Intermediate.cert.pem*) |


## <a name="configure-the-provisioning-device-code"></a>Konfigurera enhets koden för etablering

I det här avsnittet uppdaterar du exempel koden för att etablera enheten med enhets etablerings tjänst instansen. Om enheten är autentiserad tilldelas den en IoT-hubb som är länkad till Device Provisioning service-instansen.

1. I Azure Portal väljer du fliken **Översikt** för enhets etablerings tjänsten och noterar värdet för **_ID-omfång_** .

    ![Extrahera information om enhetsetableringstjänstens slutpunkt från bladet på portalen](./media/quick-create-simulated-device-x509/extract-dps-endpoints.png) 

2. I Solution Explorer för Visual Studio navigerar du till **Provisioning_Samples > prov_dev_client_sample > källfiler** och öppnar *prov_dev_client_sample. c*.

3. Hitta konstanten `id_scope` och ersätt värdet med ditt värde för **ID-omfång** som du kopierade tidigare. 

    ```c
    static const char* id_scope = "0ne00000A0A";
    ```

4. Hitta definitionen för funktionen `main()` i samma fil. Se till att `hsm_type` variabeln är inställd på `SECURE_DEVICE_TYPE_X509` som visas nedan.

    ```c
    SECURE_DEVICE_TYPE hsm_type;
    //hsm_type = SECURE_DEVICE_TYPE_TPM;
    hsm_type = SECURE_DEVICE_TYPE_X509;
    //hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;
    ```

5. Högerklicka på projektet **prov\_dev\_client\_sample** och välj **Set as Startup Project** (Ange som startprojekt).

6. På Visual Studio-menyn väljer du **Felsök**  >  **Start utan fel sökning** för att köra lösningen. När du uppmanas att återskapa projektet väljer du **Ja** för att återskapa projektet innan det körs.

    Följande utdata är ett exempel på etablerings enhetens klient exempel som har startats och ansluter till etablerings tjänsten. Enheten har tilldelats till en IoT-hubb och registrerad:

    ```cmd
    Provisioning API Version: 1.3.9
    
    Registering Device
    
    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    
    Registration Information received from service: test-docs-hub.azure-devices.net, deviceId: custom-hsm-device-01
    Press enter key to exit:
    ```

7. I portalen navigerar du till IoT-hubben som är länkad till etablerings tjänsten och väljer fliken **IoT-enheter** . Vid lyckad etablering av X. 509-enheten till hubben visas dess enhets-ID på bladet **IoT-enheter** med *status* **aktive rad**. Du kan behöva klicka på knappen **Uppdatera** längst upp. 

    ![Anpassad HSM-enhet har registrerats med IoT Hub](./media/tutorial-custom-hsm-enrollment-group-x509/hub-provisioned-custom-hsm-x509-device.png) 

## <a name="clean-up-resources"></a>Rensa resurser

När du är klar med att testa och utforska det här enhets klient exemplet kan du använda följande steg för att ta bort alla resurser som skapats i den här kursen.

1. Stäng utdatafönstret för enhetsklientexemplet på datorn.
1. Välj **alla resurser** på den vänstra menyn i Azure Portal och välj sedan enhets etablerings tjänsten. Öppna **Hantera registreringar** för din tjänst och välj sedan fliken **registrerings grupper** . Markera kryss rutan bredvid *grupp namnet* för enhets gruppen som du skapade i den här självstudien och klicka på knappen **ta bort** högst upp i fönstret. 
1. Klicka på **certifikat** i DPS. För varje certifikat som du har överfört och verifierat i den här självstudien klickar du på certifikatet och klickar på knappen **ta bort** för att ta bort det
1. Välj **alla resurser** på den vänstra menyn i Azure Portal och välj sedan din IoT Hub. Öppna **IoT-enheter** för hubben. Markera kryss rutan bredvid *enhets-ID* för den enhet som du registrerade i den här självstudien. Klicka på knappen **ta bort** högst upp i fönstret.

## <a name="next-steps"></a>Nästa steg

I den här självstudien har du allokerat en X. 509-enhet med en anpassad HSM till IoT Hub. Om du vill lära dig hur du etablerar IoT-enheter på flera hubbar fortsätter du till nästa självstudie. 

> [!div class="nextstepaction"]
> [Självstudie: etablera enheter över belastningsutjämnade IoT-hubbar](tutorial-provision-multiple-hubs.md)
