---
title: De draaiing van het certificaat voor Azure Database for MySQL
description: Meer informatie over de aanstaande wijzigingen van basis certificaat wijzigingen die van invloed zijn op Azure Database for MySQL
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: conceptual
ms.date: 01/13/2021
ms.openlocfilehash: a65ac8d52c17a288447193fb8c0fba2c6e6c5554
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2021
ms.locfileid: "98201260"
---
# <a name="understanding-the-changes-in-the-root-ca-change-for-azure-database-for-mysql"></a>Informatie over de wijzigingen in de basis-CA-wijziging voor Azure Database for MySQL

Azure Database for MySQL het basis certificaat voor de client toepassing/het stuur programma dat is ingeschakeld met SSL wijzigt, wordt gebruikt om [verbinding te maken met de database server](concepts-connectivity-architecture.md). Het basis certificaat dat momenteel beschikbaar is, is ingesteld op 15 februari 2021 (02/15/2021) als onderdeel van de aanbevolen procedures voor standaard onderhoud en beveiliging. In dit artikel vindt u meer informatie over de aanstaande wijzigingen, de bronnen die worden beïnvloed en de stappen die nodig zijn om ervoor te zorgen dat uw toepassing verbinding met uw database server onderhoudt.

>[!NOTE]
> Op basis van de feedback van klanten hebben we de afschaffing van het basis certificaat uitgebreid voor onze bestaande Baltimore-basis certificerings instantie van oktober 26, 2020 tot en met 15 februari 2021. We hopen dat deze uitbrei ding voldoende lever tijd biedt voor onze gebruikers om de client wijzigingen te implementeren als ze worden beïnvloed.

> [!NOTE]
> Oordeelloze communicatie
>
> Microsoft biedt ondersteuning voor een gevarieerde en insluitende omgeving. Dit artikel bevat verwijzingen naar de woorden _Master_ en _Slave_. In de micro soft- [stijl gids voor bias-free Communication](https://github.com/MicrosoftDocs/microsoft-style-guide/blob/master/styleguide/bias-free-communication.md) worden deze herkend als uitgesloten woorden. De woorden worden in dit artikel gebruikt voor consistentie omdat ze momenteel de woorden zijn die in de software worden weer gegeven. Wanneer de software is bijgewerkt om de woorden te verwijderen, wordt dit artikel zodanig bijgewerkt dat het in uitlijning is.
>

## <a name="what-update-is-going-to-happen"></a>Wat gebeurt er met de update?

In sommige gevallen gebruiken toepassingen een lokaal certificaat bestand dat is gegenereerd op basis van een certificaat bestand van een vertrouwde certificerings instantie (CA) om veilig verbinding te maken. Momenteel kunnen klanten alleen het vooraf gedefinieerde certificaat gebruiken om verbinding te maken met een Azure Database for MySQL-server, die [hier](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem)zich bevindt. Het [browser forum van Certificate Authority (CA)](https://cabforum.org/)   heeft echter onlangs rapporten gepubliceerd van meerdere certificaten die zijn uitgegeven door leveranciers van de certificerings instantie om niet-compatibel te zijn.

Conform de nalevings vereisten van de branche begon de leveranciers van de certificerings instantie CA-certificaten voor niet-compatibele Ca's te intrekken, waardoor het voor servers vereist dat certificaten worden gebruikt die zijn uitgegeven door compatibele Ca's en ondertekend zijn door CA-certificaten van die compatibele certificerings instanties. Omdat Azure Database for MySQL momenteel een van deze niet-conforme certificaten gebruikt, die client toepassingen gebruiken om hun SSL-verbindingen te valideren, moeten we ervoor zorgen dat de juiste acties worden ondernomen (verderop in dit onderwerp beschreven) om de potentiële impact op uw MySQL-servers tot een minimum te beperken.

Het nieuwe certificaat wordt vanaf 15 februari 2021 (02/15/2021) gebruikt. Als u een CA-validatie of volledige validatie van het server certificaat gebruikt wanneer u verbinding maakt vanaf een MySQL-client (sslmode = controleren-CA of sslmode = controleren-Full), moet u de configuratie van de toepassing bijwerken vóór 15 februari 2021 (03/15/2021).

## <a name="how-do-i-know-if-my-database-is-going-to-be-affected"></a>Hoe kan ik weet of mijn Data Base wordt beïnvloed?

Alle toepassingen die gebruikmaken van SSL/TLS en controleren of het basis certificaat het basis certificaat moet bijwerken. U kunt bepalen of uw verbindingen het basis certificaat verifiëren door uw connection string te controleren.

* Als uw connection string bevat `sslmode=verify-ca` of `sslmode=verify-identity` , moet u het certificaat bijwerken.
* Als uw Connection String bevat `sslmode=disable` , `sslmode=allow` , `sslmode=prefer` , of `sslmode=require` , hoeft u geen certificaten bij te werken.
* Als u Java-connectors gebruikt en uw connection string bevat useSSL = False of requireSSL = False, hoeft u geen certificaten bij te werken.
* Als uw connection string geen sslmode opgeeft, hoeft u geen certificaten bij te werken.

Als u een client gebruikt die de connection string wegsamen vatting, raadpleegt u de documentatie van de client om te begrijpen of de certificaten worden geverifieerd.
Bekijk de beschrijvingen van de SSL- [modus](concepts-ssl-connection-security.md#ssl-default-settings)om inzicht te krijgen in azure database for MySQL sslmode.

Als u wilt voor komen dat de beschik baarheid van uw toepassing wordt onderbroken als gevolg van een onverwacht aantal certificaten, of als u een ingetrokken certificaat wilt bijwerken, raadpleegt u de sectie [**' wat heb ik nodig om verbinding te behouden '**](concepts-certificate-rotation.md#what-do-i-need-to-do-to-maintain-connectivity) .

## <a name="what-do-i-need-to-do-to-maintain-connectivity"></a>Wat heb ik nodig om de connectiviteit in stand te houden

Voer de volgende stappen uit om te voor komen dat de beschik baarheid van uw toepassing wordt onderbroken omdat de certificaten onverwacht zijn ingetrokken of als u een ingetrokken certificaat wilt bijwerken. Het is een goed idee om een nieuw *. pem* -bestand te maken, waarin het huidige certificaat en de nieuwe worden gecombineerd en tijdens de validatie van het SSL-certificaat een van de toegestane waarden worden gebruikt. Raadpleeg de volgende stappen:

* Down load BaltimoreCyberTrustRoot & DigiCertGlobalRootG2-basis-CA van de volgende koppelingen:

  * [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem)
  * [https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem)

* Genereer een gecombineerd CA-certificaat archief met zowel **BaltimoreCyberTrustRoot** als **DigiCertGlobalRootG2** -certificaten.

  * Voor Java-gebruikers (MySQL-Connector/J) voert u de volgende handelingen uit:

    ```console
    keytool -importcert -alias MySQLServerCACert -file D:\BaltimoreCyberTrustRoot.crt.pem -keystore truststore -storepass password -noprompt
    ```

    ```console
    keytool -importcert -alias MySQLServerCACert2 -file D:\DigiCertGlobalRootG2.crt.pem -keystore truststore -storepass password -noprompt
    ```

    Vervang vervolgens het bestand van de oorspronkelijke opslag plaats door de nieuwe, gegenereerde versie:

    * System. setProperty ("javax. net. SSL. trustStore", "path_to_truststore_file");
    * System. setProperty ("javax. net. SSL. trustStorePassword", "wacht woord");

  * Voor .NET-gebruikers (MySQL-Connector/NET, MySQLConnector) moet u ervoor zorgen dat **BaltimoreCyberTrustRoot** en **DigiCertGlobalRootG2** beide bestaan in Windows-certificaat archief, vertrouwde basis certificerings instanties. Als er geen certificaten bestaan, importeert u het ontbrekende certificaat.

        ![Azure Database for MySQL .net cert](media/overview/netconnecter-cert.png)

  * Voor .NET-gebruikers op Linux met behulp van SSL_CERT_DIR, moet u ervoor zorgen dat **BaltimoreCyberTrustRoot** en **DigiCertGlobalRootG2** beide bestaan in de map die wordt aangegeven door SSL_CERT_DIR. Als er geen certificaten bestaan, maakt u het ontbrekende certificaat bestand.

  * Voor andere gebruikers (MySQL client/MySQL Workbench/C/C++/Go/Python/Ruby/PHP/NodeJS/Perl/Swift) kunt u twee CA-certificaat bestanden samen voegen in de volgende indeling:</b>

     </br>-----BEGIN CERTIFICAAT-----  </br>(Root CA1: BaltimoreCyberTrustRoot. CRT. pem)  </br>-----EIND CERTIFICAAT-----  </br>-----BEGIN CERTIFICAAT-----  </br>(Root CA2: DigiCertGlobalRootG2. CRT. pem)  </br>-----EIND CERTIFICAAT-----

* Vervang het oorspronkelijke PEM-bestand van de basis-CA door het gecombineerde basis-CA-bestand en start de toepassing/client opnieuw.
* Nadat het nieuwe certificaat op de server is geïmplementeerd, kunt u in de toekomst het PEM-bestand van de CA wijzigen in DigiCertGlobalRootG2. CRT. pem.

## <a name="what-can-be-the-impact-of-not-updating-the-certificate"></a>Wat kan de invloed zijn van het bijwerken van het certificaat?

Als u het Azure Database for MySQL verleende certificaat gebruikt zoals hier wordt beschreven, kan de beschik baarheid van uw toepassing worden onderbroken omdat de data base niet bereikbaar is. Afhankelijk van uw toepassing kunnen verschillende fout berichten worden weer gegeven, inclusief, maar niet beperkt tot,:

* Ongeldig certificaat/ingetrokken certificaat
* Time-out opgetreden voor verbinding

> [!NOTE]
> Verwijder het **Baltimore-certificaat** niet en pas het pas toe nadat het certificaat is gewijzigd. Er wordt een communicatie verzonden nadat de wijziging is aangebracht, waarna het Baltimore-certificaat door hen veilig kan worden verwijderd.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="1-if-im-not-using-ssltls-do-i-still-need-to-update-the-root-ca"></a>1. als ik SSL/TLS niet gebruik, moet ik dan nog steeds de basis-CA bijwerken?

  Er zijn geen acties vereist als u geen gebruik maakt van SSL/TLS.

### <a name="2-if-im-using-ssltls-do-i-need-to-restart-my-database-server-to-update-the-root-ca"></a>2. als ik SSL/TLS gebruik, moet ik mijn database server opnieuw starten om de basis-CA bij te werken?

Nee, u hoeft de database server niet opnieuw op te starten om het nieuwe certificaat te gebruiken. Dit basis certificaat is een wijziging aan de client zijde en de binnenkomende client verbindingen moeten het nieuwe certificaat gebruiken om ervoor te zorgen dat ze verbinding kunnen maken met de database server.

### <a name="3-what-will-happen-if-i-dont-update-the-root-certificate-before-february-15-2021-02152021"></a>3. Wat gebeurt er als ik het basis certificaat niet bijwerk vóór 15 februari 2021 (02/15/2021)?

Als u het basis certificaat niet vóór 15 februari 2021 (02/15/2021) bijwerkt, kunnen uw toepassingen die verbinding maken via SSL/TLS en verificatie voor het basis certificaat niet communiceren met de MySQL-database server en kan de toepassing verbindings problemen ondervinden met de MySQL-database server.

### <a name="4-what-is-the-impact-if-using-app-service-with-azure-database-for-mysql"></a>4. Wat is de impact van het gebruik van App Service met Azure Database for MySQL?

Voor Azure app services die verbinding maken met Azure Database for MySQL, zijn er twee mogelijke scenario's en is afhankelijk van hoe u SSL met uw toepassing gebruikt.

* Dit nieuwe certificaat is toegevoegd aan App Service op platform niveau. Als u de SSL-certificaten gebruikt die zijn opgenomen in App Service platform in uw toepassing, is er geen actie vereist.
* Als u het pad naar het SSL-certificaat bestand in uw code expliciet opneemt, moet u het nieuwe certificaat downloaden en de code bijwerken om het nieuwe certificaat te gebruiken. Een goed voor beeld van dit scenario is wanneer u aangepaste containers in App Service gebruikt zoals gedeeld in de [app service documentatie](../app-service/tutorial-multi-container-app.md#configure-database-variables-in-wordpress)

### <a name="5-what-is-the-impact-if-using-azure-kubernetes-services-aks-with-azure-database-for-mysql"></a>5. Wat is de impact van het gebruik van Azure Kubernetes Services (AKS) met Azure Database for MySQL?

Als u probeert verbinding te maken met de Azure Database for MySQL met behulp van Azure Kubernetes Services (AKS), is dit vergelijkbaar met de toegang vanuit een specifieke host-omgeving van klanten. Raadpleeg de stappen [hier](../aks/ingress-own-tls.md).

### <a name="6-what-is-the-impact-if-using-azure-data-factory-to-connect-to-azure-database-for-mysql"></a>6. Wat is de impact van Azure Data Factory om verbinding te maken met Azure Database for MySQL?

Voor een connector die gebruikmaakt van Azure Integration Runtime, maakt de connector gebruik van certificaten in het Windows-certificaat archief in de op Azure gehoste omgeving. Deze certificaten zijn al compatibel met de zojuist toegepaste certificaten en daarom is er geen actie vereist.

Voor een connector die gebruikmaakt van zelf-hostende Integration Runtime waarbij u het pad naar het SSL-certificaat bestand in uw connection string expliciet opneemt, moet u het [nieuwe certificaat](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem) downloaden en de Connection String bijwerken om het te gebruiken.

### <a name="7-do-i-need-to-plan-a-database-server-maintenance-downtime-for-this-change"></a>7. moet ik de downtime van een database server onderhoud voor deze wijziging plannen?

Nee. Aangezien de wijziging hier alleen aan de client zijde wordt weer gegeven om verbinding te maken met de database server, is er geen uitval tijd nodig voor de database server voor deze wijziging.

### <a name="8--what-if-i-cannot-get-a-scheduled-downtime-for-this-change-before-february-15-2021-02152021"></a>8. Wat moet ik doen als ik vóór 15 februari 2021 (02/15/2021) geen geplande downtime voor deze wijziging krijg?

Omdat de clients die zijn gebruikt voor het maken van een verbinding met de server, de certificaat gegevens moeten bijwerken zoals beschreven in de sectie herstellen, hoeven we in dit geval geen downtime voor de [server te gebruiken](./concepts-certificate-rotation.md#what-do-i-need-to-do-to-maintain-connectivity).

### <a name="9-if-i-create-a-new-server-after-february-15-2021-02152021-will-i-be-impacted"></a>9. als ik een nieuwe server Maak na 15 februari 2021 (02/15/2021), wordt dit van invloed?

Voor servers die zijn gemaakt na 15 februari 2021 (02/15/2021), kunt u het zojuist uitgegeven certificaat voor uw toepassingen gebruiken om verbinding te maken via SSL.

### <a name="10-how-often-does-microsoft-update-their-certificates-or-what-is-the-expiry-policy"></a>10. hoe vaak werkt micro soft hun certificaten bij of wat is het verloop beleid?

Deze certificaten die worden gebruikt door Azure Database for MySQL worden door vertrouwde certificerings instanties (CA) verschaft. Daarom is de ondersteuning van deze certificaten op Azure Database for MySQL gekoppeld aan de ondersteuning van deze certificaten per CA. In dit geval kunnen er echter onvoorziene fouten voor komen in deze vooraf gedefinieerde certificaten, die op het eerst moeten worden opgelost.

### <a name="11-if-im-using-read-replicas-do-i-need-to-perform-this-update-only-on-source-server-or-the-read-replicas"></a>11. als ik lees replica's gebruik, moet ik deze update alleen uitvoeren op de bron server of de Lees replica's?

Omdat deze update een wijziging aan de client zijde is, moet u ook de wijzigingen voor deze clients Toep assen als de client gebruikt om gegevens te lezen van de replica-server.

### <a name="12-if-im-using-data-in-replication-do-i-need-to-perform-any-action"></a>12. als ik gegevens replicatie gebruik, moet ik dan elke actie uitvoeren?

Als u [gegevens replicatie](concepts-data-in-replication.md) gebruikt om verbinding te maken met Azure database for MySQL, moet u rekening houden met twee dingen:

* Als de gegevens replicatie van een virtuele machine (on-premises of Azure virtual machine) naar Azure Database for MySQL is, moet u controleren of SSL wordt gebruikt om de replica te maken. Voer de **status van slave weer geven** uit en controleer de volgende instelling.  

    ```azurecli-interactive
    Master_SSL_Allowed            : Yes
    Master_SSL_CA_File            : ~\azure_mysqlservice.pem
    Master_SSL_CA_Path            :
    Master_SSL_Cert               : ~\azure_mysqlclient_cert.pem
    Master_SSL_Cipher             :
    Master_SSL_Key                : ~\azure_mysqlclient_key.pem
    ```

    Als u ziet dat het certificaat is ingesteld voor de CA_file, SSL_Cert en SSL_Key, moet u het bestand bijwerken door het [nieuwe certificaat](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem)toe te voegen.

* Als de gegevens replicatie tussen twee Azure Database for MySQL ligt, moet u de replica opnieuw instellen door het **aanroepen van MySQL.az_replication_change_master** uit te voeren en het nieuwe dubbele basis certificaat als laatste para meter op te geven [master_ssl_ca](howto-data-in-replication.md#link-source-and-replica-servers-to-start-data-in-replication)

### <a name="13-do-we-have-server-side-query-to-verify-if-ssl-is-being-used"></a>13. hebben we een query aan server zijde om te controleren of SSL wordt gebruikt?

Raadpleeg [SSL-verificatie](howto-configure-ssl.md#step-4-verify-the-ssl-connection)om te controleren of u SSL-verbinding gebruikt om verbinding te maken met de server.

### <a name="14-is-there-an-action-needed-if-i-already-have-the-digicertglobalrootg2-in-my-certificate-file"></a>14. is er een actie vereist als ik DigiCertGlobalRootG2 al in mijn certificaat bestand heb?

Nee. Er is geen actie nodig als uw certificaat bestand al het **DigiCertGlobalRootG2** heeft.

###    <a name="15-what-if-i-have-further-questions"></a>15. Wat moet ik doen als ik meer vragen heb?

Als u vragen hebt, kunt u antwoorden krijgen van experts van community's in [micro soft Q&A](mailto:AzureDatabaseforMySQL@service.microsoft.com). Als u een ondersteunings abonnement hebt en technische hulp nodig hebt, kunt u [contact met ons](mailto:AzureDatabaseforMySQL@service.microsoft.com)opnemen.
