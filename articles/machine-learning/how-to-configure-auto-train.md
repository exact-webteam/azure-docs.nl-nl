---
title: Geautomatiseerde ML-experimenten maken
titleSuffix: Azure Machine Learning
description: Meer informatie over het definiëren van gegevens bronnen, berekeningen en configuratie-instellingen voor uw geautomatiseerde machine learning experimenten.
author: cartacioS
ms.author: sacartac
ms.reviewer: nibaccam
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.date: 09/29/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python,contperf-fy21q1, automl
ms.openlocfilehash: 8ac69e6961af4991b250320b7af7cf5a345d3efb
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/03/2021
ms.locfileid: "99526463"
---
# <a name="configure-automated-ml-experiments-in-python"></a>Geautomatiseerde ML-experimenten configureren in Python


In deze hand leiding vindt u informatie over het definiëren van verschillende configuratie-instellingen van uw geautomatiseerde machine learning experimenten met de [Azure machine learning SDK](/python/api/overview/azure/ml/intro?preserve-view=true&view=azure-ml-py). Automatische machine learning kiest een algoritme en Hyper parameters voor u en genereert een model dat gereed is voor implementatie. Er zijn verschillende opties die u kunt gebruiken voor het configureren van geautomatiseerde machine learning experimenten.

Zie [zelf studie: een classificatie model met geautomatiseerde machine learning](tutorial-auto-train-models.md) of [Train modellen met geautomatiseerde machine learning in de Cloud](how-to-auto-train-remote.md)om voor beelden van automatische machine learning experimenten te bekijken.

Configuratie opties die beschikbaar zijn in automatische machine learning:

* Selecteer het type experiment: classificatie, regressie of time series-prognose
* Gegevens bron, indelingen en gegevens ophalen
* Kies uw reken doel: lokaal of extern
* Instellingen voor automatisch machine learning experiment
* Een geautomatiseerd machine learning-experiment uitvoeren
* Model statistieken verkennen
* Model registreren en implementeren

Als u liever geen code hebt, kunt u ook [uw geautomatiseerde machine learning experimenten maken in azure machine learning Studio](how-to-use-automated-ml-for-ml-models.md).

## <a name="prerequisites"></a>Vereisten

Voor dit artikel hebt u het volgende nodig: 
* Een Azure Machine Learning-werkruimte. Zie [een Azure machine learning-werk ruimte maken](how-to-manage-workspace.md)voor het maken van de werk ruimte.

* De Azure Machine Learning python SDK is geïnstalleerd.
    Als u de SDK wilt installeren, kunt u 
    * Maak een reken instantie, waarmee de SDK automatisch wordt geïnstalleerd en vooraf is geconfigureerd voor ML-werk stromen. Zie [een Azure machine learning Compute-exemplaar maken en beheren](how-to-create-manage-compute-instance.md) voor meer informatie. 

    * [Installeer het `automl` pakket zelf](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/README.md#setup-using-a-local-conda-environment), dat de [standaard installatie](/python/api/overview/azure/ml/install?preserve-view=true&view=azure-ml-py#default-install) van de SDK bevat.

## <a name="select-your-experiment-type"></a>Het type experimenten selecteren

Voordat u begint met het experiment, moet u het soort machine learning probleem bepalen dat u wilt oplossen. Automatische machine learning ondersteunt taak typen van `classification` , `regression` en `forecasting` . Meer informatie over [taak typen](concept-automated-ml.md#when-to-use-automl-classify-regression--forecast).

De volgende code gebruikt de `task` para meter in de `AutoMLConfig` constructor om het soort experiment op te geven als `classification` .

```python
from azureml.train.automl import AutoMLConfig

# task can be one of classification, regression, forecasting
automl_config = AutoMLConfig(task = "classification")
```

## <a name="data-source-and-format"></a>Gegevensbron en -indeling

Geautomatiseerde machine learning biedt ondersteuning voor gegevens die zich bevinden op de lokale desktop of in de cloud, zoals Azure Blob Storage. De gegevens kunnen worden gelezen in een **Panda data frame** of een **Azure machine learning TabularDataset**. [Meer informatie over gegevenssets](how-to-create-register-datasets.md).

Vereisten voor trainings gegevens in machine learning:
- Gegevens moeten in tabel vorm zijn.
- De waarde die u wilt voors pellen, doel kolom, moet in de gegevens zijn.

**Voor externe experimenten** moeten opleidings gegevens toegankelijk zijn vanaf de externe compute. AutoML accepteert alleen [Azure Machine Learning TabularDatasets](/python/api/azureml-core/azureml.data.tabulardataset?preserve-view=true&view=azure-ml-py) wanneer ze werken op een externe compute. 

Azure Machine Learning-gegevenssets bieden functionaliteit voor:

* U kunt eenvoudig gegevens van statische bestanden of URL-bronnen overdragen naar uw werk ruimte.
* Het beschikbaar maken van uw gegevens voor trainingsscripts bij uitvoering in computeresources in de cloud. Bekijk [hoe u met gegevens sets traint](how-to-train-with-datasets.md#mount-files-to-remote-compute-targets) voor een voor beeld van het gebruik van de- `Dataset` klasse om gegevens te koppelen aan uw externe Compute-doel.

Met de volgende code wordt een TabularDataset gemaakt op basis van een web-URL. Zie [een TabularDatasets maken](how-to-create-register-datasets.md#create-a-tabulardataset) voor code voorbeelden voor het maken van gegevens sets van andere bronnen, zoals lokale bestanden en gegevens opslag.

```python
from azureml.core.dataset import Dataset
data = "https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/creditcard.csv"
dataset = Dataset.Tabular.from_delimited_files(data)
  ```
**Voor lokale Compute experimenten** raden wij Pandas dataframes aan voor snellere verwerkings tijden.

  ```python
  import pandas as pd
  from sklearn.model_selection import train_test_split

  df = pd.read_csv("your-local-file.csv")
  train_data, test_data = train_test_split(df, test_size=0.1, random_state=42)
  label = "label-col-name"
  ```

## <a name="training-validation-and-test-data"></a>Trainings-, validatie-en test gegevens

U kunt afzonderlijke **trainings gegevens en validatie gegevens sets** rechtstreeks in de `AutoMLConfig` constructor opgeven. Meer informatie over [het configureren van gegevens splitsingen en kruis validatie](how-to-configure-cross-validation-data-splits.md) voor uw AutoML-experimenten. 

Als u een `validation_data` of `n_cross_validation` -para meter niet expliciet opgeeft, worden in automatische ml standaard technieken toegepast om te bepalen hoe validatie wordt uitgevoerd. Deze bepaling is afhankelijk van het aantal rijen in de gegevensset die aan uw `training_data` para meter is toegewezen. 

|Grootte van de trainings &nbsp; gegevens &nbsp;| Validatie techniek |
|---|-----|
|**Meer &nbsp; dan &nbsp; 20.000 &nbsp; rijen**| De splitsing van Train/validatie gegevens wordt toegepast. De standaard instelling is om 10% van de eerste set met trainings gegevens te nemen als de validatieset. Deze validatieset wordt vervolgens gebruikt voor de berekening van metrische gegevens.
|**Kleiner &nbsp; dan &nbsp; 20.000 &nbsp; rijen**| De methode voor kruis validatie wordt toegepast. Het standaard aantal vouwen is afhankelijk van het aantal rijen. <br> **Als de gegevensset kleiner is dan 1.000 rijen**, worden er 10 vouwen gebruikt. <br> **Als de rijen tussen 1.000 en 20.000** liggen, worden er drie vouwen gebruikt.

Op dit moment moet u uw eigen **test gegevens** voor de model evaluatie opgeven. Zie de sectie **testen** van [Dit Jupyter-notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-credit-card-fraud/auto-ml-classification-credit-card-fraud.ipynb)voor een code voorbeeld van het inbrengen van uw eigen test gegevens voor model evaluatie.

## <a name="compute-to-run-experiment"></a>Compute en uitvoering van het experiment instellen

Bepaal vervolgens op welke locatie het model moet worden getraind. Een experiment met geautomatiseerd machine learning-training kan worden uitgevoerd op de volgende compute-opties. Meer informatie over de [voordelen en nadelen van lokale en externe compute-opties](concept-automated-ml.md#local-remote). 

* Uw **lokale** computer, zoals een lokaal bureau blad of laptop, in het algemeen wanneer u een kleine gegevensset hebt en u zich nog steeds in het exploratie stadium bevindt. Bekijk [dit notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/local-run-classification-credit-card-fraud/auto-ml-classification-credit-card-fraud-local.ipynb) voor een lokaal compute-voorbeeld. 
 
* Een **externe** computer in de cloud – [Azure machine learning beheerde Compute](concept-compute-target.md#amlcompute) is een beheerde service waarmee u machine learning modellen kunt trainen op clusters van virtuele machines van Azure. 

    Raadpleeg [dit notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb) voor een extern voorbeeld met behulp van beheerde compute van Azure Machine Learning. 

* Een **Azure Databricks cluster** in uw Azure-abonnement. Meer informatie kunt u vinden in [een Azure Databricks cluster instellen voor automatische milliliters](how-to-configure-databricks-automl-environment.md). Bekijk deze [github-site](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/azure-databricks/automl) voor voor beelden van notitie blokken met Azure Databricks.

<a name='configure-experiment'></a>

## <a name="configure-your-experiment-settings"></a>De instellingen voor uw experiment configureren

Er zijn verschillende opties die u kunt gebruiken om uw geautomatiseerde machine learning-experiment te configureren. Deze para meters worden ingesteld door een object te instantiëren `AutoMLConfig` . Zie de [klasse AutoMLConfig](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig) voor een volledige lijst met para meters.

Voorbeelden zijn:

1. Beoordelings experiment waarbij AUC wordt gewogen als primaire metriek met de time-out van het experiment, ingesteld op 30 minuten en 2 Kruis validatie vouwen.

   ```python
       automl_classifier=AutoMLConfig(task='classification',
                                      primary_metric='AUC_weighted',
                                      experiment_timeout_minutes=30,
                                      blocked_models=['XGBoostClassifier'],
                                      training_data=train_data,
                                      label_column_name=label,
                                      n_cross_validations=2)
   ```
1. Het volgende voor beeld is een regressie-experiment dat is ingesteld op eindigen na 60 minuten met vijf validatie-Kruis vouwen.

   ```python
      automl_regressor = AutoMLConfig(task='regression',
                                      experiment_timeout_minutes=60,
                                      allowed_models=['KNN'],
                                      primary_metric='r2_score',
                                      training_data=train_data,
                                      label_column_name=label,
                                      n_cross_validations=5)
   ```


1. Voor prognose taken zijn extra instellingen vereist. Raadpleeg het artikel een prognose model voor het maken van [een time-reeks](how-to-auto-train-forecast.md) voor meer informatie. 

    ```python
    time_series_settings = {
        'time_column_name': time_column_name,
        'time_series_id_column_names': time_series_id_column_names,
        'forecast_horizon': n_test_periods
    }
    
    automl_config = AutoMLConfig(task = 'forecasting',
                                 debug_log='automl_oj_sales_errors.log',
                                 primary_metric='normalized_root_mean_squared_error',
                                 experiment_timeout_minutes=20,
                                 training_data=train_data,
                                 label_column_name=label,
                                 n_cross_validations=5,
                                 path=project_folder,
                                 verbosity=logging.INFO,
                                 **time_series_settings)
    ```
    
### <a name="supported-models"></a>Ondersteunde modellen

Automatische machine learning probeert verschillende modellen en algoritmen tijdens het automatiserings-en afstemmings proces. Als gebruiker hoeft u het algoritme niet op te geven. 

De drie verschillende `task` parameter waarden bepalen de lijst met algoritmen, of modellen, die moeten worden toegepast. Gebruik de `allowed_models` `blocked_models` para meters of om iteraties verder te wijzigen met de beschik bare modellen die moeten worden opgenomen of uitgesloten. 

De volgende tabel bevat een overzicht van de ondersteunde modellen op taak type. 

> [!NOTE]
> Als u van plan bent om uw automatisch gegenereerde modellen te exporteren naar een [ONNX-model](concept-onnx.md), kunnen alleen de algoritmen worden geconverteerd naar de ONNX-indeling. Meer informatie over [het converteren van modellen naar ONNX](concept-automated-ml.md#use-with-onnx). <br> <br> Opmerking: ONNX ondersteunt op dit moment alleen classificatie-en regressie taken. 

Classificatie | Regressie | Tijdreeksvoorspelling
|-- |-- |--
[Logistiek regressie](https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression)* | [Elastisch net](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)* | [Elastic Net](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)
[Licht GBM](https://lightgbm.readthedocs.io/en/latest/index.html)* |[Licht GBM](https://lightgbm.readthedocs.io/en/latest/index.html)*|[Licht GBM](https://lightgbm.readthedocs.io/en/latest/index.html)
[Kleur overgang verhogen](https://scikit-learn.org/stable/modules/ensemble.html#classification)* |[Kleur overgang verhogen](https://scikit-learn.org/stable/modules/ensemble.html#regression)* |[Kleur overgang verhogen](https://scikit-learn.org/stable/modules/ensemble.html#regression)
[Beslissings structuur](https://scikit-learn.org/stable/modules/tree.html#decision-trees)* |[Beslissings structuur](https://scikit-learn.org/stable/modules/tree.html#regression)* |[Beslissings structuur](https://scikit-learn.org/stable/modules/tree.html#regression)
[K dichtstbijzijnde neighbors](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)* |[K dichtstbijzijnde neighbors](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)* |[K dichtstbijzijnde neighbors](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)
[Lineair-SVC](https://scikit-learn.org/stable/modules/svm.html#classification)* |[LARS lasso](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)* |[LARS Lasso](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)
[Vector classificatie (SVC) ondersteunen](https://scikit-learn.org/stable/modules/svm.html#classification)* |[Stochastische kleur overgang Daal (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)* |[Stochastische kleur overgang Daal (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)
[Wille keurig forest](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)* |[Wille keurig forest](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)* |[Wille keurig forest](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)
[Zeer wille keurige structuren](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)* |[Zeer wille keurige structuren](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)* |[Zeer wille keurige structuren](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)
[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)* |[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)* | [Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)
[Gemiddelde Perceptron-classificatie](/python/api/nimbusml/nimbusml.linear_model.averagedperceptronbinaryclassifier?preserve-view=true&view=nimbusml-py-latest)|[Online verloop Daal Regressor hierop](/python/api/nimbusml/nimbusml.linear_model.onlinegradientdescentregressor?preserve-view=true&view=nimbusml-py-latest) |[Automatische ARIMA](https://www.alkaline-ml.com/pmdarima/modules/generated/pmdarima.arima.auto_arima.html#pmdarima.arima.auto_arima)
[Naive Bayes](https://scikit-learn.org/stable/modules/naive_bayes.html#bernoulli-naive-bayes)* |[Snelle lineaire Regressor hierop](/python/api/nimbusml/nimbusml.linear_model.fastlinearregressor?preserve-view=true&view=nimbusml-py-latest)|[Prophet](https://facebook.github.io/prophet/docs/quick_start.html)
[Stochastische kleur overgang Daal (SGD)](https://scikit-learn.org/stable/modules/sgd.html#sgd)* ||ForecastTCN
|[Classificatie van lineaire SVM](/python/api/nimbusml/nimbusml.linear_model.linearsvmbinaryclassifier?preserve-view=true&view=nimbusml-py-latest)*||

### <a name="primary-metric"></a>Primaire metriek
De `primary metric` para meter bepaalt de metrische gegevens die moeten worden gebruikt tijdens de model training voor Optima Lise ring. De beschik bare metrische gegevens die u kunt selecteren, worden bepaald door het taak type dat u kiest, en in de volgende tabel worden geldige primaire metrische gegevens weer gegeven voor elk taak type.

Het kiezen van een primaire metriek voor automatische machine learning om te optimaliseren, is afhankelijk van een groot aantal factoren. We raden u aan om een metrische waarde te kiezen die het beste past bij uw bedrijfs behoeften. Bedenk vervolgens of de metriek geschikt is voor uw gegevensverzamelings Profiel (gegevens grootte, bereik, klasse-distributie, enzovoort).

Meer informatie over de specifieke definities van deze metrische gegevens in [inzicht in geautomatiseerde machine learning resultaten](how-to-understand-automated-ml.md).

|Classificatie | Regressie | Tijdreeksvoorspelling
|--|--|--
|`accuracy`| `spearman_correlation` | `spearman_correlation`
|`AUC_weighted` | `normalized_root_mean_squared_error` | `normalized_root_mean_squared_error`
|`average_precision_score_weighted` | `r2_score` | `r2_score`
|`norm_macro_recall` | `normalized_mean_absolute_error` | `normalized_mean_absolute_error`
|`precision_score_weighted` |

### <a name="primary-metrics-for-classification-scenarios"></a>Primaire metrische gegevens voor classificatie scenario's 

Het boeken van een drempel waarde, zoals `accuracy` ,, `average_precision_score_weighted` `norm_macro_recall` , en `precision_score_weighted` kan mogelijk niet worden geoptimaliseerd voor gegevens sets die zeer klein zijn, hebben een zeer grote klasse scheefheid (klasse onevenwicht), of wanneer de verwachte metriekwaarde dicht bij 0,0 of 1,0 ligt. In dergelijke gevallen `AUC_weighted` kan de primaire metriek een betere keuze zijn. Nadat automatische machine learning is voltooid, kunt u het winnende model kiezen op basis van de metrische gegevens die het meest geschikt zijn voor uw bedrijfs behoeften.

| Metrisch | Voor beeld van use case (s) |
| ------ | ------- |
| `accuracy` | Afbeeldings classificatie, sentiment analyse, verloop voorspelling |
| `AUC_weighted` | Fraude detectie, afbeeldings classificatie, afwijkings detectie/detectie van spam |
| `average_precision_score_weighted` | Sentimentanalyse |
| `norm_macro_recall` | Verloop voorspelling |
| `precision_score_weighted` |  |

### <a name="primary-metrics-for-regression-scenarios"></a>Primaire metrische gegevens voor regressie scenario's

Metrische gegevens zoals `r2_score` en `spearman_correlation` kunnen de kwaliteit van het model beter weer geven als de schaal van de waarde-voor-voor speling een groot aantal orders van omvang omvat. Voor een schatting van het exemplaar van het salaris, waarbij veel mensen een salaris hebben van $20.000 naar $100.000, maar de schaal erg hoog is bij sommige Sala rissen in het bereik $100 miljoen. 

`normalized_mean_absolute_error` en `normalized_root_mean_squared_error` zou in dit geval een $20.000-Voorspellings fout behandelen voor een werk nemer met een $30k-salaris als een werk nemer die $20 miljoen maakt. In werkelijkheid is het voors pellen van slechts $20.000 uit een $20 miljoen-salaris erg dicht (een klein 0,1% relatief verschil), terwijl $20.000 uit $30k niet sluiten is (een groot verschil van 67% ten opzichte van €). `normalized_mean_absolute_error` en `normalized_root_mean_squared_error` zijn nuttig wanneer de te voors pellen waarden op een vergelijk bare schaal zijn.

| Metrisch | Voor beeld van use case (s) |
| ------ | ------- |
| `spearman_correlation` | |
| `normalized_root_mean_squared_error` | Prijs voorspelling (House/product/tip), voor spelling van Score controleren |
| `r2_score` | Vertraging van de luchtvaart maatschappij, schatting van het salaris, oplossings tijd van de fout |
| `normalized_mean_absolute_error` |  |

### <a name="primary-metrics-for-time-series-forecasting-scenarios"></a>Primaire metrische gegevens voor prognose scenario's voor time series

Zie regressie notities hierboven.

| Metrisch | Voor beeld van use case (s) |
| ------ | ------- |
| `spearman_correlation` | |
| `normalized_root_mean_squared_error` | Prijs voorspelling (prognose), voorraad optimalisatie, vraag prognose |
| `r2_score` | Prijs voorspelling (prognose), voorraad optimalisatie, vraag prognose |
| `normalized_mean_absolute_error` | |

### <a name="data-featurization"></a>Gegevens parametrisatie

In elk automatisch machine learning experiment worden uw gegevens automatisch geschaald en genormaliseerd om *bepaalde* algoritmen te helpen die gevoelig zijn voor functies die op verschillende schalen zijn. Deze schaal en normalisatie worden parametrisatie genoemd. Zie [parametrisatie in AutoML](how-to-configure-auto-features.md#) voor meer details en code voorbeelden. 

Wanneer u uw experimenten in uw `AutoMLConfig` object configureert, kunt u de instelling in-of uitschakelen `featurization` . De volgende tabel bevat de geaccepteerde instellingen voor parametrisatie in het [AutoMLConfig-object](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig). 

|Parametrisatie-configuratie | Description |
| ------------- | ------------- |
|`"featurization": 'auto'`| Geeft aan dat als onderdeel van de voor verwerking, [gegevens Guardrails en parametrisatie-stappen](how-to-configure-auto-features.md#featurization) automatisch worden uitgevoerd. **Standaard instelling**.|
|`"featurization": 'off'`| Hiermee wordt aangegeven dat de parametrisatie-stap niet automatisch moet worden uitgevoerd.|
|`"featurization":`&nbsp;`'FeaturizationConfig'`| Hiermee wordt aangegeven dat er een aangepaste parametrisatie-stap moet worden gebruikt. [Meer informatie over het aanpassen van parametrisatie](how-to-configure-auto-features.md#customize-featurization).|

> [!NOTE]
> Automatische machine learning parametrisatie stappen (functie normalisatie, het verwerken van ontbrekende gegevens, het converteren van tekst naar numerieke waarde, enzovoort) worden onderdeel van het onderliggende model. Wanneer u het model gebruikt voor voor spellingen, worden dezelfde parametrisatie-stappen die tijdens de training worden toegepast, automatisch toegepast op de invoer gegevens.

<a name="ensemble"></a>

### <a name="ensemble-configuration"></a>Configuratie van ensemble

Ensemble-modellen zijn standaard ingeschakeld en worden weer gegeven als de laatste uitvoerings herhalingen in een AutoML-uitvoering. Momenteel worden **VotingEnsemble** en **StackEnsemble** ondersteund. 

Met stemmen worden zachte stem geïmplementeerd, waarbij gewogen gemiddelden worden gebruikt. De stacking-implementatie maakt gebruik van een implementatie met twee lagen, waarbij de eerste laag dezelfde modellen heeft als de juiste ensemble en het tweede laag model wordt gebruikt om de optimale combi natie van modellen van de eerste laag te vinden. 

Als u gebruikmaakt van ONNX-modellen **of** als u model uitleg hebt ingeschakeld, wordt de stacking uitgeschakeld en wordt alleen stem gebruik gebruikgemaakt.

Een ensemble-training kan worden uitgeschakeld met behulp van de `enable_voting_ensemble` `enable_stack_ensemble` para meters en Booleaanse waarden.

```python
automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        experiment_timeout_minutes=30,
        training_data=data_train,
        label_column_name=label,
        n_cross_validations=5,
        enable_voting_ensemble=False,
        enable_stack_ensemble=False
        )
```

Voor het wijzigen van het standaard gedrag van ensembles zijn er meerdere standaard argumenten die kunnen worden gegeven als `kwargs` in een- `AutoMLConfig` object.

> [!IMPORTANT]
>  De volgende para meters zijn geen expliciete para meters van de klasse AutoMLConfig. 

* `ensemble_download_models_timeout_sec`: Tijdens het genereren van het **VotingEnsemble** -en **StackEnsemble** -model worden er meerdere modellen van de vorige onderliggende uitvoeringen gedownload. Als deze fout optreedt: `AutoMLEnsembleException: Could not find any models for running ensembling` , moet u mogelijk meer tijd opgeven om de modellen te downloaden. De standaard waarde is 300 seconden voor het gelijktijdig downloaden van deze modellen en er is geen maximale time-outlimiet. Configureer deze para meter met een hogere waarde dan 300 seconden, als er meer tijd nodig is. 

  > [!NOTE]
  >  Als de time-out is bereikt en er modellen zijn gedownload, wordt de ensembling-out voortgezet met zo veel modellen die zijn gedownload. Het is niet vereist dat alle modellen moeten worden gedownload om binnen die time-out te volt ooien.

De volgende para meters zijn alleen van toepassing op **StackEnsemble** -modellen: 

* `stack_meta_learner_type`: de meta-informatieer is een model dat is getraind op de uitvoer van de afzonderlijke heterogene modellen. Standaard-meta informatie is `LogisticRegression` voor classificatie taken (of `LogisticRegressionCV` als kruis validatie is ingeschakeld) en `ElasticNet` voor regressie-en prognose taken (of `ElasticNetCV` als kruis validatie is ingeschakeld). Deze para meter kan een van de volgende teken reeksen zijn:,,,,, `LogisticRegression` `LogisticRegressionCV` `LightGBMClassifier` `ElasticNet` `ElasticNetCV` `LightGBMRegressor` , of `LinearRegression` .

* `stack_meta_learner_train_percentage`: Hiermee geeft u het aandeel van de Trainingsset (bij het kiezen van trein en validatie type training) op dat moet worden gereserveerd voor de training van de meta-informatieer. De standaardwaarde is `0.2`. 

* `stack_meta_learner_kwargs`: optionele para meters die moeten worden door gegeven aan de initialisatie functie van de meta-informatieer. Met deze para meters en parameter typen worden de para meters en parameter typen van de bijbehorende model-constructor gespiegeld en doorgestuurd naar de model-constructor.

De volgende code toont een voor beeld van het opgeven van een aangepast ensemble-gedrag in een `AutoMLConfig` object.

```python
ensemble_settings = {
    "ensemble_download_models_timeout_sec": 600
    "stack_meta_learner_type": "LogisticRegressionCV",
    "stack_meta_learner_train_percentage": 0.3,
    "stack_meta_learner_kwargs": {
        "refit": True,
        "fit_intercept": False,
        "class_weight": "balanced",
        "multi_class": "auto",
        "n_jobs": -1
    }
}

automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        experiment_timeout_minutes=30,
        training_data=train_data,
        label_column_name=label,
        n_cross_validations=5,
        **ensemble_settings
        )
```

<a name="exit"></a> 

### <a name="exit-criteria"></a>Afsluit criteria

Er zijn enkele opties die u in uw AutoMLConfig kunt definiëren om uw experiment te beëindigen.

|Criteria| beschrijving
|----|----
Geen &nbsp; criteria | Als u geen afsluit parameters definieert, wordt het experiment voortgezet tot er geen verdere voortgang wordt gemaakt op basis van uw primaire metriek.
Na &nbsp; een &nbsp; &nbsp; tijds duur &nbsp;| Gebruik `experiment_timeout_minutes` in uw instellingen om te bepalen hoe lang, in minuten, uw experiment moet blijven werken. <br><br> Om te voor komen dat er storingen optreden, is er mini maal 15 minuten of 60 minuten als uw rij op kolom grootte groter is dan 10.000.000.
Er &nbsp; is een score &nbsp; &nbsp; &nbsp; bereikt| Gebruik `experiment_exit_score` voltooit het experiment nadat een opgegeven primaire meet Score is bereikt.

## <a name="run-experiment"></a>Experiment uitvoeren

Voor automatische MILLILITERs maakt u een `Experiment` -object dat een benoemd object is in een `Workspace` gebruikt om experimenten uit te voeren.

```python
from azureml.core.experiment import Experiment

ws = Workspace.from_config()

# Choose a name for the experiment and specify the project folder.
experiment_name = 'Tutorial-automl'
project_folder = './sample_projects/automl-classification'

experiment = Experiment(ws, experiment_name)
```

Dien het experiment in om een model uit te voeren en te genereren. Geef de `AutoMLConfig` methode door aan `submit` om het model te genereren.

```python
run = experiment.submit(automl_config, show_output=True)
```

>[!NOTE]
>Afhankelijkheden worden eerst op een nieuwe computer geïnstalleerd.  Het kan tot tien minuten duren voordat uitvoer wordt weer gegeven.
>Instelling `show_output` `True` van resultaten die in uitvoer wordt weer gegeven op de-console.

### <a name="multiple-child-runs-on-clusters"></a>Meerdere onderliggende uitvoeringen op clusters

Automatische ML experimenten-onderliggende uitvoeringen kunnen worden uitgevoerd op een cluster waarop al een ander experiment wordt uitgevoerd. De timing is echter afhankelijk van het aantal knoop punten dat het cluster heeft, en als deze knoop punten beschikbaar zijn voor het uitvoeren van een ander experiment.

Elk knoop punt in het cluster fungeert als een afzonderlijke virtuele machine (VM) die een enkele training kan uitvoeren. voor automatische MILLILITERs betekent dit dat een onderliggende run. Als alle knoop punten bezet zijn, wordt het nieuwe experiment in de wachtrij geplaatst. Als er echter gratis knoop punten zijn, voert het nieuwe experiment automatische ML onderliggende uitvoeringen parallel uit in de beschik bare knoop punten/Vm's.

Voor het beheren van onderliggende uitvoeringen en wanneer ze kunnen worden uitgevoerd, raden we u aan een toegewezen cluster per experiment te maken en het aantal `max_concurrent_iterations` van uw experiment te vergelijken met het aantal knoop punten in het cluster. Op deze manier gebruikt u alle knoop punten van het cluster tegelijk met het aantal gelijktijdige onderliggende uitvoeringen/iteraties dat u wilt.

`max_concurrent_iterations`In uw `AutoMLConfig` object configureren. Als deze niet is geconfigureerd, wordt standaard slechts één gelijktijdig onderliggend item/herhaling per experiment toegestaan.  

## <a name="explore-models-and-metrics"></a>Modellen en metrische gegevens verkennen

U kunt uw trainings resultaten weer geven in een widget of inline als u zich in een notebook bevindt. Zie [modellen volgen en evalueren](how-to-monitor-view-training-logs.md#monitor-automated-machine-learning-runs) voor meer informatie.

Bekijk de [resultaten van automatische machine learning experimenten evalueren](how-to-understand-automated-ml.md) voor definities en voor beelden van de prestatie diagrammen en metrische gegevens die voor elke uitvoering worden geleverd. 

Zie [parametrisatie transparantie](how-to-configure-auto-features.md#featurization-transparency)voor een parametrisatie samen vatting en inzicht in de functies die zijn toegevoegd aan een bepaald model. 

> [!NOTE]
> De algoritmen die automatisch worden geautomatiseerd, hebben een inherente wille keurigheid waardoor kleine afwijkingen in de Score van de uiteindelijke metrische gegevens van een aanbevolen model, zoals nauw keurigheid, kunnen ontstaan. Automatische ML voert ook bewerkingen uit op gegevens zoals de splitsing van Train-test, gesplitste validatie of Kruis validatie wanneer dat nodig is. Als u dus een experiment met dezelfde configuratie-instellingen en primaire gegevens meerdere keren uitvoert, zult u waarschijnlijk de variatie van elke eindmetrieke meet waarde van elke experimenten zien als gevolg van deze factoren. 

## <a name="register-and-deploy-models"></a>Modellen registreren en implementeren

Zie [hoe en wanneer u een model wilt implementeren](how-to-deploy-and-where.md)voor meer informatie over het downloaden of registreren van een model voor implementatie naar een webservice.

<a name="explain"></a>

## <a name="model-interpretability"></a>Interpreteerbaarheid van modellen

Met de functie voor het interpreteren van modellen kunt u begrijpen waarom de voor spellingen en de onderliggende waarden van de functie belang rijk zijn. De SDK bevat verschillende pakketten voor het inschakelen van functies voor het interpreteren van modellen, zowel tijdens trainingen als voor het afleiden van tijd, voor lokale en geïmplementeerde modellen.

Raadpleeg de [procedures](how-to-machine-learning-interpretability-automl.md) voor code voorbeelden voor het inschakelen van de functies voor het door verwijzen van voorzieningen, specifiek binnen automatische machine learning experimenten.

Voor algemene informatie over hoe model toelichtingen en functie belang kunnen worden ingeschakeld op andere gebieden van de SDK buiten automatische machine learning, raadpleegt u het [concept](how-to-machine-learning-interpretability.md) artikel over de interpretatie mogelijkheden.

> [!NOTE]
> Het ForecastTCN-model wordt momenteel niet ondersteund door de uitleg-client. Dit model retourneert geen uitzonderings dashboard als het wordt geretourneerd als het beste model en biedt geen ondersteuning voor uitleg over de uitvoering van een on-demand.

## <a name="troubleshooting"></a>Problemen oplossen

* De **recente upgrade van `AutoML` afhankelijkheden naar nieuwere versies leidt tot een afbreuk aan de compatibiliteit**: vanaf versie 1.13.0 van de SDK worden modellen niet geladen in oudere sdk's vanwege incompatibiliteit tussen de oudere versies die we in onze vorige pakketten hebben vastgemaakt en de nieuwere versies die we nu vastmaken. U ziet de fout, zoals:
  * Module niet gevonden: ex. `No module named 'sklearn.decomposition._truncated_svd` ,
  * Import fouten: ex. `ImportError: cannot import name 'RollingOriginValidator'` ,
  * Kenmerk fouten: ex. `AttributeError: 'SimpleImputer' object has no attribute 'add_indicator`
  
  U kunt dit probleem omzeilen door een van de volgende twee stappen uit te voeren, afhankelijk van de `AutoML` SDK-trainings versie:
    * Als uw `AutoML` SDK-trainings versie hoger is dan 1.13.0, hebt u `pandas == 0.25.1` en nodig `scikit-learn==0.22.1` . Als er sprake is van een versie die niet overeenkomt, upgradet u scikit-Learn en/of Pandas naar de juiste versie, zoals hieronder wordt weer gegeven:
      
      ```bash
         pip install --upgrade pandas==0.25.1
         pip install --upgrade scikit-learn==0.22.1
      ```
      
    * Als uw `AutoML` SDK-trainings versie lager is dan of gelijk is aan 1.12.0, hebt u `pandas == 0.23.4` en nodig `sckit-learn==0.20.3` . Als er een versie conflict is, downgrade scikit-Learn en/of Pandas om de versie te corrigeren, zoals hieronder wordt weer gegeven:
  
      ```bash
        pip install --upgrade pandas==0.23.4
        pip install --upgrade scikit-learn==0.20.3
      ```

* **Mislukte implementatie**: voor versies <= 1.18.0 van de SDK kan de basis installatie kopie die is gemaakt voor de implementatie mislukken met de volgende fout: "ImportError: kan geen naam importeren `cached_property` uit `werkzeug` ". 

  De volgende stappen kunnen het probleem omzeilen:
  1. Het model pakket downloaden
  2. Pak het pakket uit
  3. Implementeren met behulp van de uitgepakte assets

* **Forecasting R2-Score is altijd nul**: dit probleem doet zich voor als de verstrekte trainings gegevens een tijd reeks hebben die dezelfde waarde voor de laatste `n_cv_splits`  +  `forecasting_horizon` gegevens punten bevat. Als dit patroon in uw tijd reeks wordt verwacht, kunt u de primaire metriek overschakelen naar een genormaliseerd wortel fout.
 
* **Tensor flow**: vanaf versie 1.5.0 van de SDK installeert automatische machine learning standaard geen tensor flow-modellen. Als u tensor flow wilt installeren en wilt gebruiken met uw geautomatiseerde ML experimenten, installeert u tensor flow = = 1.12.0 via CondaDependecies. 
 
   ```python
   from azureml.core.runconfig import RunConfiguration
   from azureml.core.conda_dependencies import CondaDependencies
   run_config = RunConfiguration()
   run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['tensorflow==1.12.0'])
  ```

* **Experimenteer grafieken**: binaire classificatie grafieken (Precision-intrekken, Roc, winst curve enz.) die worden weer gegeven in automatische ml-experimenten, worden niet correct gerenderd in de gebruikers interface sinds 4/12. In grafiek grafieken worden momenteel inverse resultaten weer gegeven, waarbij betere modellen worden weer gegeven met lagere resultaten. Een oplossing wordt onderzocht.

* **Databricks annuleren automatische uitvoering van een machine learning**: wanneer u gebruikmaakt van geautomatiseerde machine learning mogelijkheden op Azure Databricks, kunt u een uitvoering annuleren en een nieuwe uitvoering van het experiment starten door het Azure Databricks-cluster opnieuw op te starten.

* **Databricks >tien herhalingen voor automatische machine learning** machine learning: als u meer dan 10 iteraties hebt, moet u instellen `show_output` op `False` Wanneer u de uitvoering verzendt.

* **Databricks-widget voor de Azure machine learning SDK en geautomatiseerde machine learning**: de SDK-widget Azure machine learning wordt niet ondersteund in een Databricks-notebook omdat de notebooks geen HTML-widgets kunnen parseren. U kunt de widget in de portal weer geven met behulp van deze python-code in uw Azure Databricks notebook-cel:

    ```
    displayHTML("<a href={} target='_blank'>Azure Portal: {}</a>".format(local_run.get_portal_url(), local_run.id))
    ```
* **automl_setup mislukt**: 
    * Voer automl_setup uit vanaf een Anaconda-prompt in Windows. Gebruik deze koppeling om [Miniconda te installeren](https://docs.conda.io/en/latest/miniconda.html).
    * Zorg ervoor dat de Conda 64-bits is geïnstalleerd in plaats van 32-bits door de opdracht uit te voeren `conda info` . De `platform` moet `win-64` voor Windows of `osx-64` voor Mac zijn.
    * Zorg ervoor dat Conda 4.4.10 of hoger is geïnstalleerd. U kunt de versie controleren met de opdracht `conda -V` . Als u een vorige versie hebt geïnstalleerd, kunt u deze bijwerken met behulp van de opdracht: `conda update conda` .
    * Spreek `gcc: error trying to exec 'cc1plus'`
      *  Als de `gcc: error trying to exec 'cc1plus': execvp: No such file or directory` fout is opgetreden, installeert u build Essentials met behulp van de opdracht `sudo apt-get install build-essential` .
      * Geef een nieuwe naam als de eerste para meter op automl_setup om een nieuwe Conda-omgeving te maken. Bekijk bestaande Conda-omgevingen met `conda env list` en verwijder ze met `conda env remove -n <environmentname>` .
      
* **automl_setup_linux. sh mislukt**: als automl_setup_linus. sh mislukt bij Ubuntu Linux met de fout: `unable to execute 'gcc': No such file or directory`-
  1. Zorg ervoor dat de uitgaande poorten 53 en 80 zijn ingeschakeld. Op een virtuele machine van Azure kunt u dit doen vanuit de Azure Portal door de virtuele machine te selecteren en op netwerken te klikken.
  2. Voer de volgende opdracht uit: `sudo apt-get update`
  3. Voer de volgende opdracht uit: `sudo apt-get install build-essential --fix-missing`
  4. `automl_setup_linux.sh`Opnieuw uitvoeren

* **Configuration. ipynb mislukt**:
  * Voor lokale Conda moet u er eerst voor zorgen dat automl_setup is uitgevoerd.
  * Zorg ervoor dat de subscription_id juist is. Zoek de subscription_id in het Azure Portal door alle services te selecteren en vervolgens op abonnementen. De tekens ' < ' en ' > ' mogen niet worden opgenomen in de subscription_id waarde. `subscription_id = "12345678-90ab-1234-5678-1234567890abcd"`Heeft bijvoorbeeld een geldige indeling.
  * Zorg ervoor dat Inzender of eigenaar toegang heeft tot het abonnement.
  * Controleer of de regio een van de ondersteunde regio's is: `eastus2` , `eastus` , `westcentralus` , `southeastasia` , `westeurope` , `australiaeast` , `westus2` , `southcentralus` .
  * Zorg ervoor dat u toegang tot de regio hebt met behulp van de Azure Portal.
  
* **`import AutoMLConfig` mislukt**: er zijn pakket wijzigingen in de geautomatiseerde machine learning versie 1.0.76. hiervoor moet de vorige versie worden verwijderd voordat de nieuwe versie kan worden bijgewerkt. Als de `ImportError: cannot import name AutoMLConfig` is aangetroffen na een upgrade van een SDK-versie vóór v 1.0.76 naar v 1.0.76 of hoger, lost u de fout op door het volgende uit te voeren: `pip uninstall azureml-train automl` en vervolgens `pip install azureml-train-auotml` . Het script automl_setup. cmd doet dit automatisch. 

* **Workspace.from_config mislukt**: als de aanroepen ws = Workspace.from_config () mislukken
  1. Controleer of de configuratie. ipynb-notebook is uitgevoerd.
  2. Als het notitie blok wordt uitgevoerd vanuit een map die zich niet in de map bevindt waar de `configuration.ipynb` was uitgevoerd, kopieert u de map aml_config en het bestand config.jsop dat item zich in de nieuwe map. Workspace.from_config leest de config.jsop voor de notitieblokmap of de bovenliggende map.
  3. Als er een nieuw abonnement, een resource groep, werk ruimte of regio wordt gebruikt, moet u ervoor zorgen dat u het `configuration.ipynb` notitie blok opnieuw uitvoert. Het is niet mogelijk om config.jsrechtstreeks te wijzigen als de werk ruimte al bestaat in de opgegeven resource groep onder het opgegeven abonnement.
  4. Als u de regio wilt wijzigen, wijzigt u de werk ruimte, de resource groep of het abonnement. `Workspace.create` Er wordt geen werk ruimte gemaakt of bijgewerkt als deze al bestaat, zelfs als de opgegeven regio verschillend is.
  
* **Voorbeeld notitieblok mislukt**: als een voor beeld van een notebook mislukt met een fout die eigenschap, methode of bibliotheek niet bestaat:
  * Zorg ervoor dat de juiste kernel is geselecteerd in de Jupyter Notebook. De kernel wordt weer gegeven in de rechter bovenhoek van de notitie blok pagina. De standaard waarde is azure_automl. De kernel wordt opgeslagen als onderdeel van het notitie blok. Als u overschakelt naar een nieuwe Conda-omgeving, moet u dus de nieuwe kernel in het notitie Blok selecteren.
      * Voor Azure Notebooks moet het python 3,6 zijn. 
      * Voor lokale Conda-omgevingen moet dit de Conda-omgevings naam zijn die u hebt opgegeven in automl_setup.
  * Zorg ervoor dat het notitie blok voor de SDK-versie is die u gebruikt. U kunt de SDK-versie controleren door `azureml.core.VERSION` in een Jupyter notebook-cel uit te voeren. U kunt de vorige versie van de voorbeeld notitieblokken downloaden van GitHub door op de knop te klikken `Branch` , het tabblad te selecteren `Tags` en vervolgens de versie te selecteren.

* **`import numpy` mislukt in Windows**: sommige Windows-omgevingen zien een fout bij het laden van numpy met de meest recente python-versie 3.6.8. Als u dit probleem ziet, probeert u met python-versie 3.6.7.

* **`import numpy` mislukt**: Controleer de tensor flow-versie in de Automated ml Conda-omgeving. Ondersteunde versies zijn < 1,13. Verwijder tensor flow uit de omgeving als de versie >= 1,13 is. U kunt de versie van tensor flow controleren en de installatie als volgt verwijderen:
  1. Start een opdracht shell, activeer de Conda-omgeving waar automatisch ml-pakketten worden geïnstalleerd.
  2. Voer `pip freeze` in en zoek naar `tensorflow` , indien gevonden, de weer gegeven versie moet < 1,13
  3. Als de vermelde versie geen ondersteunde versie is, `pip uninstall tensorflow` typt u in de opdracht shell en voert u y in voor bevestiging.
  
 * **Uitvoeren mislukt met `jwt.exceptions.DecodeError`**: exact fout bericht: `jwt.exceptions.DecodeError: It is required that you pass in a value for the "algorithms" argument when calling decode()` .

    Voor versies <= 1.17.0 van de SDK kan de installatie leiden tot een niet-ondersteunde versie van PyJWT. Controleer de PyJWT-versie in de Automated ml Conda-omgeving. Ondersteunde versies zijn < 2.0.0. U kunt de versie van PyJWT als volgt controleren:
    1. Start een opdracht shell, activeer de Conda-omgeving waar automatisch ml-pakketten worden geïnstalleerd.
    2. Voer `pip freeze` in en zoek naar `PyJWT` , indien gevonden, de weer gegeven versie < 2.0.0

    Als de vermelde versie geen ondersteunde versie is:
    1. U kunt een upgrade uitvoeren naar de nieuwste versie van de AutoML SDK: `pip install -U azureml-sdk[automl]` .
    2. Als dat niet haalbaar is, verwijdert u PyJWT uit de omgeving en installeert u de juiste versie als volgt:
        - `pip uninstall PyJWT` in de opdracht shell en voert u in `y` voor bevestiging.
        - Installeren met `pip install 'PyJWT<2.0.0'` .

## <a name="next-steps"></a>Volgende stappen

+ Meer informatie over [hoe en waar een model moet worden geïmplementeerd](how-to-deploy-and-where.md).

+ Meer informatie over [het trainen van een regressie model met geautomatiseerde machine learning](tutorial-auto-train-models.md) of [hoe u het gebruik van geautomatiseerde machine learning op een externe bron kunt trainen](how-to-auto-train-remote.md).

+ Meer informatie over het trainen van meerdere modellen met AutoML in de [vele modellen oplossings versneller](https://aka.ms/many-models).
