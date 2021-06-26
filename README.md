# Machine Learning Model Development using TensorFlow Extended(TFX)

## ExampleGen
The ExampleGen TFX Pipeline component ingests data into TFX pipelines. It consumes external files/services to generate Examples which will be read by other TFX components. 

## The StatisticsGen TFX Pipeline Component
The StatisticsGen TFX pipeline component generates features statistics over both training and serving data, which can be used by other pipeline components. StatisticsGen uses Beam to scale to large datasets.
* Consumes: datasets created by an ExampleGen pipeline component.
* Emits: Dataset statistics.

## The SchemaGen TFX Pipeline Component
Some TFX components use a description of your input data called a schema. The schema is an instance of schema.proto. It can specify data types for feature values, whether a feature has to be present in all examples, allowed value ranges, and other properties. A SchemaGen pipeline component will automatically generate a schema by inferring types, categories, and ranges from the training data.

* Consumes: statistics from a StatisticsGen component
* Emits: Data schema proto

