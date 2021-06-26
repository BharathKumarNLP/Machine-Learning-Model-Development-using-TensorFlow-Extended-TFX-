# Machine Learning Model Development using TensorFlow Extended(TFX)

## ExampleGen
The ExampleGen TFX Pipeline component ingests data into TFX pipelines. It consumes external files/services to generate Examples which will be read by other TFX components. 

## The StatisticsGen TFX Pipeline Component
The StatisticsGen TFX pipeline component generates features statistics over both training and serving data, which can be used by other pipeline components. StatisticsGen uses Beam to scale to large datasets.
* Consumes: datasets created by an ExampleGen pipeline component.
* Emits: Dataset statistics.



