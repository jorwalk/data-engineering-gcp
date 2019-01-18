Cloud TPU: Qwik Start
=====================

Overview
--------

Training and running deep learning models can be computationally demanding. Tensor Processing Unit (TPU) lets you train and run machine learning models faster than ever. TPU powers several Google Cloud products, including [Translate](https://ai.googleblog.com/2016/09/a-neural-network-for-machine.html), [Photos](https://www.blog.google/products/google-vr/google-lens-real-time-answers-questions-about-world-around-you/),[Search](https://searchengineland.com/meet-rankbrain-google-search-results-234386), [Assistant](https://deepmind.com/blog/wavenet-launches-google-assistant/), and [Gmail](https://www.blog.google/products/gmail/subject-write-emails-faster-smart-compose-gmail/), and is helping businesses access accelerator technology to speed up their machine learning workloads on Google Cloud.

This lab uses the [Cloud TPU Provisioning Utility](https://github.com/tensorflow/tpu/tree/master/tools/ctpu) (ctpu) as a simple tool for setting up and managing your Cloud TPU from Cloud Shell. You will learn to use a Cloud TPU to accelerate specific TensorFlow machine learning workloads on Compute Engine.

Create a Cloud Storage bucket
-----------------------------

You need a Cloud Storage bucket to store the data that you use to train your machine learning model and the results of the training.

1.  Click on Storage on the GCP Console.

2.  Click Create bucket, specifying the following options:

    -   A universally unique name of your choosing.

    -   Default storage class: `Regional`

    -   Location: `us-central1`

1.  Click Create.

Use the TPU Tool
----------------

The ctpu tool is pre-installed in your Cloud Shell.

Run the following in Cloud Shell to check your ctpu configuration:

```
ctpu print-config

```

You should see a message like this:

```
ctpu configuration:
        name: [your TPU's name]
        project: [your-project-name]
        zone: us-central1-b
If you would like to change the configuration for a single command invocation, please use the command line flags.

```

In the output message, the *name* is the name of your TPU resource (defaults to your username) and *zone* is the default geographic zone for your Compute Engine. You can change these when you run [ctpu up](https://cloud.google.com/tpu/docs/quickstart#create_resources) to create a Compute Engine VM and a Cloud TPU.

Take a look at the ctpu commands:

```
ctpu

```

Create a Compute Engine VM and a Cloud TPU
------------------------------------------

Run the following command to set up a Compute Engine virtual machine (VM) and a Cloud TPU with associated services. This combination of resources and services is called a "Cloud TPU flock":

```
ctpu up  --zone us-central1-b

```

Note: You can see all the available top-level flags with `ctpu flags`

You should see a message like this:

```
ctpu will use the following configuration:

   Name: [your TPU's name]
   Zone: us-central1-b
   GCP Project: [your project's name]
   TensorFlow Version: <set after TPU API enabled>
   VM:
     Machine Type: n1-standard-2
     Disk Size: 250 GB
     Preemptible: false
   Cloud TPU:
     Size: v2-8
     Preemptible: false

```

Press y to create your Cloud TPU resources.

The `ctpu up` command performs the following tasks:

-   Enables the Compute Engine and Cloud TPU services.
-   Creates a Compute Engine VM with the latest stable TensorFlow version pre-installed. The default zone is us-central1-b.
-   Creates a Cloud TPU with the corresponding version of TensorFlow, and passes the name of the Cloud TPU to the Compute Engine VM as an environment variable (TPU_NAME).
-   Ensures your Cloud TPU has access to resources it needs from your GCP project, by granting specific IAM roles to your Cloud TPU service account.
-   Performs a number of other checks.
-   Logs you in to your new Compute Engine VM.

Note: The first time you run ctpu up on a project, it takes longer than usual because it needs to perform additional tasks including SSH key propagation and API turn-up.

Enter Y again to continue setting up your rsa keypair, and then press Enter twice to skip setting a password.

### Verify your Compute Engine VM
When the ctpu up command has finished executing, verify that your shell prompt has changed from username@project to username@tpuname. This change shows that you are now logged into your Compute Engine VM. From this point on, you will be running your commands on the Compute Engine VM instance.

### Run a TensorFlow computation
Next you will use Cloud TPU to execute a simple TensorFlow script which performs A*X+Y.

Open a text editor (this command uses pico) and create a new Python script named cloud-tpu.py:
```
pico cloud-tpu.py
```

Copy this sample script and add it to the file, then save the file:
```
import os
import tensorflow as tf
from tensorflow.contrib import tpu
from tensorflow.contrib.cluster_resolver import TPUClusterResolver

def axy_computation(a, x, y):
  return a * x + y

inputs = [
    3.0,
    tf.ones([3, 3], tf.float32),
    tf.ones([3, 3], tf.float32),
]

tpu_computation = tpu.rewrite(axy_computation, inputs)

tpu_grpc_url = TPUClusterResolver(
    tpu=[os.environ['TPU_NAME']]).get_master()

with tf.Session(tpu_grpc_url) as sess:
  sess.run(tpu.initialize_system())
  sess.run(tf.global_variables_initializer())
  output = sess.run(tpu_computation)
  print(output)
  sess.run(tpu.shutdown_system())

print('Done!')
```

> Note: This example code allows you to test that your VM is properly configured and can run computations on Cloud TPU. To train your models, the recommended method is to use the TPUEstimator framework. Read the tutorial for running a residual network on Cloud TPU to see an example of training using TPUEstimator.

Run the TensorFlow program:
```
python cloud-tpu.py
```

The program creates a tf.Session() element with a gRPC pointing to your Cloud TPU endpoint and runs a simple computation.
```
(Output)

    [array([[4., 4., 4.],
           [4., 4., 4.],
           [4., 4., 4.]], dtype=float32)]
    Done!
```
> Note: Depending on the version of Python, you might see some RuntimeWarning messages before the output shown below. These can be safely ignored.
