---
title: Opmerkingen bij de release van Azure HDInsight
description: Nieuwste opmerkingen bij de release voor Azure HDInsight. Bekijk ontwikkel tips en Details voor Hadoop, Spark, R Server, Hive en meer.
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/08/2021
ms.openlocfilehash: 1a0b1a0400ae3d43817921e8a336421aee35ccd6
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2021
ms.locfileid: "100378142"
---
# <a name="azure-hdinsight-release-notes"></a>Opmerkingen bij de release van Azure HDInsight

Dit artikel bevat informatie over de **meest recente** updates voor Azure HDInsight-release. Zie voor meer informatie over eerdere versies het [HDInsight Release Notes-archief](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Samenvatting

Azure HDInsight is een van de populairste services van zakelijke klanten voor open-source analyses op Azure.

Als u zich wilt abonneren op release opmerkingen, kijkt u naar releases op [deze github-opslag plaats](https://github.com/hdinsight/release-notes/releases).

## <a name="release-date-02052021"></a>Release datum: 02/05/2021

Deze versie is van toepassing op zowel HDInsight 3,6 als HDInsight 4,0. HDInsight-release wordt beschikbaar gesteld voor alle regio's over enkele dagen. De release datum geeft hier de release datum van de eerste regio aan. Als de onderstaande wijzigingen niet worden weer gegeven, wacht u tot de release over enkele dagen in uw regio actief is.

## <a name="new-features"></a>Nieuwe functies
### <a name="dav4-series-support"></a>Ondersteuning voor Dav4-Series
HDInsight heeft Dav4-Series-ondersteuning toegevoegd in deze versie. Meer informatie over de [Dav4-serie](https://docs.microsoft.com/azure/virtual-machines/dav4-dasv4-series).

### <a name="kafka-rest-proxy-ga"></a>Kafka REST-proxy GA 
Met Kafka REST proxy kunt u met uw Kafka-cluster communiceren via een REST API via HTTPS. Kafka rest proxy is algemeen verkrijgbaar vanaf deze release. Meer informatie over [Kafka rest proxy vindt u hier](https://docs.microsoft.com/azure/hdinsight/kafka/rest-proxy).

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Verplaatsen naar schaal sets voor virtuele Azure-machines
HDInsight maakt nu gebruik van virtuele machines van Azure om het cluster in te richten. De service wordt geleidelijk naar virtuele- [machine schaal sets van Azure](../virtual-machine-scale-sets/overview.md)gemigreerd. Het hele proces kan maanden duren. Nadat uw regio's en abonnementen zijn gemigreerd, worden nieuw gemaakte HDInsight-clusters uitgevoerd op virtuele-machine schaal sets zonder klant acties. Er wordt geen breuk wijziging verwacht.

## <a name="deprecation"></a>Afschaffing
### <a name="disabled-vm-sizes"></a>Uitgeschakelde VM-grootten
Formulier wordt gestart 9 2021. HDInsight blokkeert alle klanten die clusters maken met behulp van standand_A8, standand_A9, standand_A10 en standand_A11 VM-grootten. Bestaande clusters worden uitgevoerd. Overweeg om over te stappen op HDInsight 4,0 om mogelijke onderbreking van systeem/ondersteuning te voor komen.

## <a name="behavior-changes"></a>Gedrags wijzigingen
### <a name="default-cluster-vm-size-changes-to-ev3-series"></a>Standaard grootte van VM-cluster verandert in Ev3-serie 
De standaard grootte van virtuele cluster-vm's wordt gewijzigd van D-serie naar Ev3-Series. Deze wijziging is van toepassing op hoofd knooppunten en worker-knoop punten. Geef de VM-grootten op die u wilt gebruiken in de ARM-sjabloon om te voor komen dat deze wijziging invloed heeft op uw geteste werk stromen.

### <a name="network-interface-resource-not-visible-for-clusters-running-on-azure-virtual-machine-scale-sets"></a>De netwerk interface bron is niet zichtbaar voor clusters die worden uitgevoerd op virtuele-machine schaal sets van Azure
HDInsight wordt geleidelijk gemigreerd naar virtuele-machine schaal sets van Azure. Netwerk interfaces voor virtuele machines zijn niet meer zichtbaar voor klanten voor clusters die gebruikmaken van virtuele-machine schaal sets van Azure.


### <a name="breaking-change-for-net-for-apache-spark-100"></a>Belang rijke wijziging voor .NET voor Apache Spark 1.0.0
Met de meest recente release introduceert HDInsight de eerste officiële versie v-1.0.0 van de bibliotheek [' .net for Apache Spark '](https://github.com/dotnet/spark) . Het biedt data frame API-volledigheid voor Spark 2.4. x en Spark 3.0. x, samen met een host van [andere functies](https://github.com/dotnet/spark/blob/master/docs/release-notes/1.0.0/release-1.0.0.md). Als er wijzigingen worden aangebracht voor deze primaire versie, raadpleegt u [de .net for Apache Spark-migratie handleiding](https://github.com/dotnet/spark/blob/master/docs/migration-guide.md#upgrading-from-microsoftspark-0x-to-10) voor informatie over de stappen die nodig zijn om uw code en pijp lijnen bij te werken. Raadpleeg deze [.net for Apache Spark v 1.0 in azure HDInsight-hand leiding](https://docs.microsoft.com/azure/hdinsight/spark/spark-dotnet-version-update#using-net-for-apache-spark-v10-in-hdinsight)voor meer informatie.


## <a name="upcoming-changes"></a>Aanstaande wijzigingen
De volgende wijzigingen worden uitgevoerd in toekomstige releases.

### <a name="default-cluster-version-will-be-changed-to-40"></a>De standaard versie van het cluster wordt gewijzigd in 4,0
Vanaf februari 2021 wordt de standaard versie van het HDInsight-cluster gewijzigd van 3,6 in 4,0. Zie [beschik bare versies](./hdinsight-component-versioning.md#available-versions)voor meer informatie over de beschik bare versies. Meer informatie over wat er nieuw is in [HDInsight 4,0](./hdinsight-version-release.md).

### <a name="os-version-upgrade"></a>Upgrade van besturingssysteem versie
Bij HDInsight wordt de versie van het besturings systeem bijgewerkt van Ubuntu 16,04 naar 18,04. De upgrade wordt voltooid vóór 2021 april.

### <a name="hdinsight-36-end-of-support-on-june-30-2021"></a>HDInsight 3,6 end of support op 30 2021 juni
HDInsight 3,6 wordt beëindigd. Het formulier wordt gestart 30 2021, klanten kunnen geen nieuwe HDInsight 3,6-clusters maken. Bestaande clusters worden uitgevoerd zonder de ondersteuning van micro soft. Overweeg om over te stappen op HDInsight 4,0 om mogelijke onderbreking van systeem/ondersteuning te voor komen.

## <a name="bug-fixes"></a>Opgeloste fouten
HDInsight blijft de betrouw baarheid en prestaties van het cluster verbeteren. 

## <a name="component-version-change"></a>Onderdeel versie wijzigen
Er is geen wijziging van de onderdeel versie voor deze versie. In [dit document](./hdinsight-component-versioning.md)vindt u de huidige versie van de onderdelen voor hdinsight 4,0 en hdinsight 3,6.
