# Qwiklab 1: Introduction to Containers and Docker


## Introduction
This lab is simply modified to use a simple beam_python wordcount.py as sample code.

Containers are a way of isolating programs or processes from each other. The primary aim of containers is to make programs easy to deploy in a way that doesn't cause them to break.

In this lab, create and run a simple Docker container image that includes a simple wordcount written in Python, upload it to a Google Container Registry, so it can be run anywhere that supports Docker.


## Project Files
In addition to the data files, the project workspace includes 3 files:

**1. /wordcount/Dockerfile**  Describe an existing Docker image library/beam_python that has beam_python pre-installed.<br>
**2. /wordcount/wordcount.py** A simple word count pipeline designed with Apache Beam Python<br>
**3. README.md**  Provides project info<br>

## Run and Distribute Containers With Docker

Docker provides a simple means to package applications as containers with a repeatable execution environment.

### Preparation

***Pull a prebuilt SDK container image***
```
docker pull apache/beam_python3.7_sdk
```
For more info, please check [**Beam SDK container images**](https://beam.apache.org/documentation/runtime/environments/)

***Look at the wordcount/Dockerfile.***<br>
```
FROM apache/beam_python3.7_sdk:latest
COPY wordcount.py /
ENTRYPOINT python /wordcount.py
```
***Look at the wordcount/wordcount.py.***<br>
```
from __future__ import print_function
import apache_beam as beam
from apache_beam.options.pipeline_options import PipelineOptions

with beam.Pipeline(options=PipelineOptions()) as p:
    lines = 
    (
        p 
        | 'Create' >> beam.Create(['cat dog', 'snake cat elephant mouse', 'dog sheep mouse elephant'])     
    )

    counts = 
    (
        lines
        | 'Split' >> (beam.FlatMap(lambda x: x.split(' ')).with_output_types("unicode"))
        | 'PairWithOne' >> beam.Map(lambda x: (x, 1))
        | 'GroupAndSum' >> beam.CombinePerKey(sum)
    )

    counts | 'Print' >> beam.ParDo(lambda x: print(x))
```

### Build a Docker image

```
$ export GCP_PROJECT=`gcloud config list core/project --format='value(core.project)'`
$ docker build -t gcr.io/${GCP_PROJECT}/wordcount:v1 .

Sending build context to Docker daemon  3.584kB
Step 1/3 : FROM apache/beam_python3.7_sdk:latest
 ---> 54563da0ef03
Step 2/3 : COPY wordcount.py /
 ---> Using cache
 ---> 068eb1d96b24
Step 3/3 : ENTRYPOINT python /wordcount.py
 ---> Using cache
 ---> e041e14359ce
Successfully built e041e14359ce
Successfully tagged gcr.io/my-first-gcp-project-271812/wordcount:v1
```

### Push a Docker image to GCR
Configure Docker to use gcloud as a Container Registry credential helper (you are only required to do this once).
```
$ PATH=/usr/lib/google-cloud-sdk/bin:$PATH
$ gcloud auth configure-docker
```

Push the image to gcr.io.
```
$ docker push gcr.io/${GCP_PROJECT}/wordcount:v1
```

### Run the Wordcount Image From Any Machine
```
$ docker run gcr.io/${GCP_PROJECT}/wordcount:v1

('cat', 2)
('dog', 2)
('snake', 1)
('elephant', 2)
('mouse', 2)
('sheep', 1)
```

To learn more about Dockerfiles, look at [**this reference.**](https://docs.docker.com/engine/reference/builder/)<br>
To know more about Docker images, look at [**this reference.**](https://docs.docker.com/storage/storagedriver/)
## Author
Xingya Zhou


