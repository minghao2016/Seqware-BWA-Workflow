[![Build Status](https://travis-ci.org/ICGC-TCGA-PanCancer/Seqware-BWA-Workflow.svg?branch=develop)](https://travis-ci.org/ICGC-TCGA-PanCancer/Seqware-BWA-Workflow)
[![Docker Repository on Quay](https://quay.io/repository/collaboratory/seqware-bwa-workflow/status "Docker Repository on Quay")](https://quay.io/repository/collaboratory/seqware-bwa-workflow)

# SeqWare BWA Workflow

## Overview

This is the SeqWare workflow for the TCGA/ICGC PanCancer project that aligns
whole genome sequences with BWA-Mem.  It also reads/writes to GNOS, the metadata/data
repository system used in the project.
For more information about the workflow see the [CHANGELOG](CHANGELOG.md).

For more information about the project overall see the
[PanCancer wiki space](https://wiki.oicr.on.ca/display/PANCANCER/PANCANCER+Home).

More detailed documentation about the production use of this workflow can be
found in the [PanCancer-Info](https://github.com/ICGC-TCGA-PanCancer/pancancer-info)
project where we maintain our production documentation and SOPs.

## Building the Workflow

This workflow (like other SeqWare workflows) is built using Maven.  In the
workflow directory (workflow-bwa-pancancer), execute the following:

    mvn clean install

This will take a long time on first build since it download dependencies from Maven
and the reference genome which is 5GB+ in size.

## Building the Worklfow Docker Image

You can also build a Docker image that has the workflow ready to run in it.

    docker build -t pancancer/pcawg-bwa-workflow:2.6.6 .

## Testing


You can run the image and then run the workflow

    docker run -ti --rm pancancer/pcawg-bwa-workflow:2.6.6 /bin/bash
    seqware bundle launch --dir target/Workflow_Bundle_BWA_2.6.6_SeqWare_1.1.1/ --engine whitestar --no-metadata

To save time, if you have reference data handy, you can mount it into the container to save time. You can grab it from the links directory when running the test execution of the workflow like above.  

    docker run -ti --rm -v /<your custom location>/links.bak:/home/seqware/Seqware-BWA-Workflow/links  pancancer/pcawg-bwa-workflow:2.6.6 /bin/bash


## Installation & Running

This is beyond the scope of this README.  Instead, you can see the SeqWare project pages for information on installing and running the workflow.  Briefly, you need a VM (local on VirtualBox, on a cloud like AWS, or a private cloud like OpenStack) to run this workflow.  We have pre-made VMs for Amazon and VirtualBox.  For other environments you can use [Bindle](https://github.com/CloudBindle/Bindle) to create a VM or cluster of VMs that are capable of running this workflow. See:

* [Setting up a SeqWare VM or Running in a Cloud](http://seqware.github.io/docs/2-installation/)
* [Testing, Installing, and Running a SeqWare Workflow](http://seqware.github.io/docs/3-getting-started/)

Feel free to email our [mailing list](http://seqware.github.io/community/) if you have questions.

## Sample Data

Some synthetic sample data.

* https://s3.amazonaws.com/oicr.workflow.bundles/released-bundles/synthetic_bam_for_GNOS_upload/hg19.chr22.5x.normal2.bam
* https://s3.amazonaws.com/oicr.workflow.bundles/released-bundles/synthetic_bam_for_GNOS_upload/hg19.chr22.5x.normal.bam

## Reference Data

We use a specific reference based on GRCh37.

* http://s3.amazonaws.com/pan-cancer-data/pan-cancer-reference/genome.fa.gz
* http://s3.amazonaws.com/pan-cancer-data/pan-cancer-reference/genome.fa.gz.fai
* http://s3.amazonaws.com/pan-cancer-data/pan-cancer-reference/genome.fa.gz.64.amb
* http://s3.amazonaws.com/pan-cancer-data/pan-cancer-reference/genome.fa.gz.64.ann
* http://s3.amazonaws.com/pan-cancer-data/pan-cancer-reference/genome.fa.gz.64.bwt
* http://s3.amazonaws.com/pan-cancer-data/pan-cancer-reference/genome.fa.gz.64.pac
* http://s3.amazonaws.com/pan-cancer-data/pan-cancer-reference/genome.fa.gz.64.sa

## Authors

* Brian O'Connor <boconnor@oicr.on.ca>
* Junjun Zhang <Junjun.Zhang@oicr.on.ca>
* Adam Wright <adam.wright@oicr.on.ca>

## Contributors

* Keiran Raine: PCAP-Core and BWA-Mem workflow design
* Roshaan Tahir: Original BWA-Align workflow design

## Workflow Authors' Release Checklist

Make sure you:

* update the workflow version in:
    * workflow.properties
    * pom.xml
    * gnos\_upload\_data.pl
    * make sure you grep for any other files
* update the description of the workflow in workflow.properties and gnos\_upload\_data.pl, this includes differences with the previous release. Update the CHANGELOG which should contain the bulk of documentation about changes and links to our Bug system.
* test the workflow locally (VM) and at clouds
* do not package your gnostest.pem key!
* release in Github using HubFlow
* upload the workflow zip to S3
