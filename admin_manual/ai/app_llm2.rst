======================================
App: Local large language model (llm2)
======================================

.. _ai-app-llm2:

The *llm2* app is one of the apps that provide text processing functionality using Large language models in Nextcloud and act as a text processing backend for the :ref:`Nextcloud Assistant app<ai-app-assistant>`, the *mail* app and :ref:`other apps making use of the core Text Processing API<tp-consumer-apps>`. The *llm2* app specifically runs only open source models and does so entirely on-premises. Nextcloud can provide customer support upon request, please talk to your account manager for the possibilities.

This app uses `ctransformers <https://github.com/marella/ctransformers>`_ under the hood and is thus compatible with any model in *gguf* format. Output quality will differ depending on which model you use, we recommend the following models:

* `Llama3 8b Instruct <https://huggingface.co/QuantFactory/Meta-Llama-3-8B-Instruct-GGUF>`_ (reasonable quality; fast; good acclaim; multilingual output may not be optimal)
* `Llama3 70B Instruct <https://huggingface.co/QuantFactory/Meta-Llama-3-70B-Instruct-GGUF>`_ (good quality; good acclaim; good multilingual output)

This app supports input and output in languages other than English if the underlying model supports the language.

Requirements
------------

* This app is built as an External App and thus depends on AppAPI v2.3.0 or higher
* Nextcloud AIO is supported
* Using GPU processing is supported, but not required; be prepared for slow performance unless you are using GPU
* We currently only support NVIDIA GPUs
* GPU Sizing

   * You will need a GPU with enough VRAM to hold the model you choose

      * Some examples:

         * for 8B parameter models, 5bit-quantized variants and lower should fit on a 8GB VRAM, but of course have slightly lower quality
         * for 8B parameter models, 6bit-quantized variants and up will need 12GB VRAM
         * for a 70B parameter model 4bit-quantized variants will need ~44GB VRAM

      * If you want better reasoning capabilities, you will need to look for models with more parameters, like 14B and higher, which of course also need more VRAM
      * To check whether a model fits on your GPU, you can use this `calculator <https://rahulschand.github.io/gpu_poor/>`_

* CPU Sizing

   * The more cores you have and the more powerful the CPU the better, we recommend 10-20 cores
   * The app will hog all cores by default, so it is usually better to run it on a separate machine

Installation
------------

0. Make sure the :ref:`Nextcloud Assistant app<ai-app-assistant>` is installed
1. :ref:`Install AppAPI and setup a Deploy Demon<ai-app_api>`
2. Install the "Local large language model" ExApp via the "External Apps" page in the Nextcloud web admin user interface

Supplying alternate models
~~~~~~~~~~~~~~~~~~~~~~~~~~

This app allows supplying alternate LLM models as *gguf* files in the ``/nc_app_llm2_data`` directory of the docker container.

1. Download a **gguf** model e.g. from huggingface
2. Copy the **gguf** file to ``/nc_app_llm2_data`` inside the docker container
3. Restart the llm2 ExApp
4. Select the new model in the Nextcloud AI admin settings

Scaling
-------

It is currently not possible to scale this app, we are working on this. Based on our calculations an instance has a rough capacity of 1000 user requests per hour. However, this number is based on theory and we do appreciate real-world feedback on this.

App store
---------

You can also find the app in our app store, where you can write a review: `<https://apps.nextcloud.com/apps/llm2>`_

Repository
----------

You can find the app's code repository on GitHub where you can report bugs and contribute fixes and features: `<https://github.com/nextcloud/llm2>`_

Nextcloud customers should file bugs directly with our Support system.

Known Limitations
-----------------

* We currently only support languages that the underlying model supports; correctness of language use in languages other than English may be poor depending on the language's coverage in the model's training data (We recommended model Llama 3 or other models explicitly trained on multiple languages)
* Language models can be bad at reasoning tasks
* Language models are likely to generate false information and should thus only be used in situations that are not critical. It's recommended to only use AI at the beginning of a creation process and not at the end, so that outputs of AI serve as a draft for example and not as final product. Always check the output of language models before using it.
* Make sure to test the language model you are using it for whether it meets the use-case's quality requirements
* Language models notoriously have a high energy consumption, if you want to reduce load on your server you can choose smaller models or quantized models in excahnge for lower accuracy
* Customer support is available upon request, however we can't solve false or problematic output, most performance issues, or other problems caused by the underlying model. Support is thus limited only to bugs directly caused by the implementation of the app (connectors, API, front-end, AppAPI)
* Due to technical limitations that we are in the process of mitigating, each task currently incurs a time cost of between 0 and 5 minutes in addition to the actual processing time
