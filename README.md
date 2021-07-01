## Machine Learning Model Development using TensorFlow Extended(TFX)
![image](https://user-images.githubusercontent.com/23136710/124017377-51ef9900-d9ef-11eb-993b-f97738ce10ed.png)

### TensorFlow Extended (TFX) 
TensorFlow Extended (TFX) is a Google-production-scale machine learning platform based on TensorFlow. TensorFlow Extended (TFX) is an end-to-end platform for deploying production ML pipelines.</br>
A TFX pipeline is a sequence of components that implement an ML pipeline which is specifically designed for scalable, high-performance machine learning tasks. Components are built using TFX libraries which can also be used individually.

### ML Metadata
ML Metadata (MLMD) is a library for recording and retrieving metadata associated with ML developer and data scientist workflows. MLMD is an integral part of TensorFlow Extended (TFX), but is designed so that it can be used independently.
### Understanding TFX Pipelines
MLOps is the practice of applying DevOps practices to help automate, manage, and audit machine learning (ML) workflows. ML workflows include steps to:
* Prepare, analyze, and transform data.
* Train and evaluate a model.
*  Deploy trained models to production.
Track ML artifacts and understand their dependencies.
### TFX Orchestrators
Orchestrators automates task executions and monitors TF components. One of the largest TFX Orchestrators is Apache Beam. Apache Beam is the unified batch and stream distributed API which acts as an abstraction layer to run on top of the distributed processing framework. This allows you to work on diverse backends such as Apache Spark, Local, Dataflow, etc.
### The StatisticsGen TFX pipeline component
The ExampleGen TFX Pipeline component ingests data into TFX pipelines. It consumes external files/services to generate Examples which will be read by other TFX components. 

### The StatisticsGen TFX Pipeline Component
The StatisticsGen TFX pipeline component generates features statistics over both training and serving data, which can be used by other pipeline components. StatisticsGen uses Beam to scale to large datasets.
* Consumes: datasets created by an ExampleGen pipeline component.
* Emits: Dataset statistics.

### The SchemaGen TFX Pipeline Component
Some TFX components use a description of your input data called a schema. The schema is an instance of schema.proto. It can specify data types for feature values, whether a feature has to be present in all examples, allowed value ranges, and other properties. A SchemaGen pipeline component will automatically generate a schema by inferring types, categories, and ranges from the training data.
* Consumes: statistics from a StatisticsGen component
* Emits: Data schema proto

### The ExampleValidator TFX Pipeline Component
The ExampleValidator pipeline component identifies anomalies in training and serving data. It can detect different classes of anomalies in the data. For example it can: 
* perform validity checks by comparing data statistics against a schema that codifies expectations of the user
* detect training-serving skew by comparing training and serving data.
* detect data drift by looking at a series of data.

The ExampleValidator pipeline component identifies any anomalies in the example data by comparing data statistics computed by the StatisticsGen pipeline component against a schema. The inferred schema codifies properties which the input data is expected to satisfy, and can be modified by the developer.
* Consumes: A schema from a SchemaGen component, and statistics from a StatisticsGen component.
* Emits: Validation results

### The Transform TFX Pipeline Component
The Transform TFX pipeline component performs feature engineering on tf.Examples emitted from an ExampleGen component, using a data schema created by a SchemaGen component, and emits both a SavedModel as well as statistics on both pre-transform and post-transform data. When executed, the SavedModel will accept tf.Examples emitted from an ExampleGen component and emit the transformed feature data.
* Consumes: tf.Examples from an ExampleGen component, and a data schema from a SchemaGen component.
* Emits: A SavedModel to a Trainer component, pre-transform and post-transform statistics.

### The Pusher TFX Pipeline Component
The Pusher component is used to push a validated model to a deployment target during model training or re-training. Before the deployment, Pusher relies on one or more blessings from other validation components to decide whether to push the model or not.

* Evaluator blesses the model if the new trained model is "good enough" to be pushed to production.
* (Optional but recommended) InfraValidator blesses the model if the model is mechanically servable in a production environment.

### The InfraValidator TFX Pipeline Component
InfraValidator is a TFX component that is used as an early warning layer before pushing a model into production. The name "infra" validator came from the fact that it is validating the model in the actual model serving "infrastructure". If Evaluator is to guarantee the performance of the model, InfraValidator is to guarantee the model is mechanically fine and prevents bad models from being pushed.

