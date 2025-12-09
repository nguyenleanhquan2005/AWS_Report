---
title: "Blog 3"
date: "2025-09-09"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---
# Maximizing EC2 Spot Instance reliability for Nextflow on AWS Batch with Memory Machine Batch

by Gabriela Karina Paulus, PhD and Christian Kniep on 18 SEP 2025 in [Amazon EC2](https://aws.amazon.com/blogs/publicsector/category/compute/amazon-ec2/), [AWS Batch](https://aws.amazon.com/blogs/publicsector/category/compute/aws-batch/), [AWS Partner Network](https://aws.amazon.com/blogs/publicsector/category/aws-partner-network/), [Life Sciences](https://aws.amazon.com/blogs/publicsector/category/industries/life-sciences/), [Nonprofit](https://aws.amazon.com/blogs/publicsector/category/public-sector/nonprofit/), [Partner solutions](https://aws.amazon.com/blogs/publicsector/category/post-types/partner-solutions/), [Public Sector](https://aws.amazon.com/blogs/publicsector/category/public-sector/), [Public Sector Partners](https://aws.amazon.com/blogs/publicsector/category/public-sector/public-sector-partners/), [Technical How-to](https://aws.amazon.com/blogs/publicsector/category/post-types/technical-how-to/) [Permalink](https://aws.amazon.com/blogs/publicsector/maximizing-ec2-spot-instance-reliability-for-nextflow-on-aws-batch-with-memory-machine-batch/)  [Share](https://aws.amazon.com/blogs/publicsector/maximizing-ec2-spot-instance-reliability-for-nextflow-on-aws-batch-with-memory-machine-batch/#)

![AWS branded background with text "Maximizing EC2 Spot Instance reliability for Nextflow on AWS Batch with Memory Machine Batch"](/images/image1_03.png)

[Amazon Web Services (AWS)](https://aws.amazon.com/) provides a comprehensive suite of services that enable life science organizations to run complex genomics workflows at scale. Through years of supporting cutting-edge research institutions, AWS has developed deep expertise in genomics computing, offering solutions that combine high performance with cost efficiency. At the heart of many genomics pipelines lies [AWS Batch](https://aws.amazon.com/batch/), a fully managed compute service that, when paired with [Amazon Elastic Compute Cloud (Amazon EC2)](https://aws.amazon.com/ec2/) Spot Instances, delivers powerful and cost-effective infrastructure for running Nextflow pipelines. By relying on this foundation, researchers can focus on scientific discovery rather than infrastructure management. To further enhance this robust platform, AWS partners with providers of innovative solutions such as [MemVerge](https://aws.amazon.com/marketplace/seller-profile?id=3b7c724c-fae7-4187-ae45-de1625e51395), an AWS Partner Network (APN) Advanced Technology Partner, whose Memory Machine Batch (MMBatch) technology complements AWS Batch by providing advanced checkpointing capabilities for [EC2 Spot Instances](https://aws.amazon.com/ec2/spot/). This collaboration exemplifies the AWS commitment to offering customers choice and flexibility in optimizing their research workflows.

Research institutes need to be on a constant lookout for how to reduce computational costs. Amazon EC2 Spot Instances are a great option for high-volume, noninteractive scientific workflows. With up to 90 percent cost savings compared to on-demand pricing, Spot Instances are attractive for high-throughput processing on a budget, but by default they are mainly suitable for fault-tolerant workloads because they can be reclaimed at any time if the asking price goes over the offered price.

However, it’s important to understand that Spot Instance cost savings apply to compute time rather than workflow progress. When Spot Instances are reclaimed, running tasks are interrupted, which can result in lost computation time—potentially hours of processing in long-running genomics workflows. While AWS Batch provides automated retry mechanisms for interrupted tasks, frequent Spot Instance reclamations can impact overall job completion times and make per-job costs less predictable. Organizations need to carefully evaluate their time-to-results requirements and implement appropriate strategies to manage these interruptions effectively.

This unpredictability has a lot of centers sticking to On-Demand Instances because they can’t risk incomplete analyses or missed deadlines. This experience isn’t unique. Many Nextflow users have encountered the challenges of working with Spot Instances when trying to balance cost-efficiency with reliability in cloud-based genomics workflows.

In this post, we look at how MemVerge’s technology can overcome the challenges of interrupted pipelines by enabling pipelines that were interrupted mid task to start from where they left off, regardless of Spot Instance reclaims. We’ll also show how MemVerge’s Batch Viewer and proven best practices empower researchers to visualize their workloads, identify bottlenecks, and apply smart strategies that make their pipelines more efficient, resilient, and cloud-optimized.

## **The trade-off between cost and reliability in EC2 Spot Instances**

For research teams working under tight budget constraints, Spot Instances offer a compelling proposition—access to the same compute capacity as EC2 On-Demand Instances, but at a fraction of the cost. It’s a powerful way to stretch grant funding, accelerate time to discovery, and process more samples without compromising scientific goals.

But using Spot Instances comes with a catch.

Spot Instances can be reclaimed by AWS with a 2-minute warning, depending on overall demand for capacity. In practice, this means that compute resources can vanish mid task, and long-running processes can fail without warning. Although Nextflow is designed to retry failed tasks, constant interruptions mean rerunning entire processes, increasing total execution time, and wasting already-consumed compute resources.

Eventually, centers shift critical stages of their workflows back to EC2 On-Demand Instances just to gain the reliability required to meet deadlines. While a shift works, it incurs steep cost—undoing the budgetary advantages that first motivated the move to the cloud.

This situation reveals a fundamental tension: Using Spot Instances has the potential to provide a steep discount for compute resources, but it doesn’t necessarily align with the requirements of the compute workflow that is orchestrating the processing power. For bioinformatics workloads that run for hours or days, that mismatch can be costly.

So how can research teams continue using EC2 Spot Instances without compromising reliability?

Within this ecosystem, MemVerge’s MMBatch serves as a complementary tool, adding transparent, container-level checkpointing to further enhance reliability.

## **How Nextflow helps—but has limits**

Nextflow is designed to integrate seamlessly with cloud-native execution platforms, and when running on AWS, it hands off task execution to AWS Batch. This decouples workflow logic from the underlying compute layer, enabling AWS Batch to take full responsibility for managing retries, scaling resources, and reacting to Spot Instance reclaims.

Within AWS Batch retry strategies are used to control what happens to a job in certain circumstances. Common strategies are to retry the job when the host had an issue—for example, the Spot instance got reclaimed by AWS due to a surge in demand from on-demand customers.

When a Spot Instance is reclaimed, AWS Batch will requeue the job until the maximum number of job attempts are reached (up to 10). From Nextflow’s perspective, the task is simply retried—no manual intervention needed. This approach is elegant because it lets AWS Batch apply cloud-specific strategies and optimizations without burdening the workflow engine. It’s a smart division of labor: Nextflow defines the workflow and AWS Batch handles the execution.

However, although this model improves reliability, it has a critical weakness: By default, every retry starts the job from scratch. The progress made in the previous attempt is usually lost.

For short tasks, that’s not a big deal. But for longer-running steps—such as aligning whole genomes or joint-calling across large sample sets—the compute loss can be massive. A task that runs for 6 hours and gets interrupted in hour 5 will still need to restart from the beginning. If Spot Instance reclaims are frequent, the cost and delay multiply rapidly, undermining the budgetary advantage of using Spot Instances in the first place.

That’s where MemVerge’s MMBatch steps in to change the game.

MMBatch doesn’t replace or override AWS Batch—it augments the way containers are launched within AWS Batch compute environments. Once integrated, MMBatch enables checkpointing of running jobs directly at the container level.

When AWS Batch reschedules a job—such as after a Spot Instance reclaim—MMBatch steps in during the container startup phase. Before the job begins executing again, MMBatch checks whether a checkpoint exists. If one is found, it automatically restores the container from that snapshot, picking up from the last saved state rather than starting over. From the AWS Batch perspective, nothing changes; it still sees a job being retried. But now, that retry isn’t a full do-over—it’s a fast recovery.

The result is powerful: The compute time already invested isn’t lost, even in the face of Spot Instance evictions. Pipelines become dramatically more resilient, long-running tasks are far less vulnerable, and the economics of Spot Instances become viable again—even for workflows that used to require on-demand reliability.

## **Visualizing and optimizing with MMBatch WebUI**

While checkpointing brings a huge leap in resilience, understanding and optimizing how pipelines behave in the cloud is just as important. That’s where the MMBatch Viewer comes in—a powerful UI that gives researchers and pipeline engineers real-time visibility into their workloads.

For many teams, cloud execution can feel like a black box. Tasks run remotely, on some instances, with variable costs and occasional failures. When things go wrong—or even just slower than expected—debugging becomes time-consuming and often guesswork-driven.

MMBatch WebUI changes that by making the execution layer transparent.

With MMBatch WebUI, users can:

* Visualize job timelines to see how long tasks actually take, where retries happen, and how Spot Instance reclaims affect progress.  
* Track checkpoint activity, identifying which jobs benefit from snapshotting and how much compute is being saved. For example, a checkpoint interval of 15 minutes doesn’t checkpoint short running jobs and thus reduces the checkpoint pressure.  
* Analyze cost and efficiency trends, helping teams pinpoint expensive or wasteful stages and refine their resource requests.  
* Diagnose bottlenecks quickly, inspect log files or download a log bundle to open a support ticket.

For the genomics research center, this visibility was a turning point. Instead of reacting to failed runs or spiraling costs, they could proactively tune their Nextflow configurations and AWS Batch strategies. They discovered which processes should be prioritized for checkpointing, adjusted memory and vCPU allocations based on real data, and even experimented with instance diversification strategies to reduce Spot Instance reclaim frequency.

Combined with MemVerge’s checkpointing, MMBatch WebUI helped them transform Spot Instances from a risky gamble into a cost-efficient, high-throughput foundation for their sequencing work.

## **Making EC2 Spot Instances work for you**

Cloud-scale genomics doesn’t have to mean choosing between cost and reliability. Nextflow and AWS Batch provide a solid foundation—but when EC2 Spot Instance reclaims threaten important deadlines, tools that go beyond retries are needed.

With MemVerge’s MMBatch, checkpointing makes even long-running jobs resilient to interruptions. And with the MMBatch WebUI, you gain the visibility and insight needed to tune your workflows for efficiency and cost-effectiveness.

For research teams running large-scale, time-sensitive workloads, these tools unlock the full potential of EC2 Spot Instances—without the tradeoffs.

The AWS advantage in research computing stems from years of experience supporting the world’s most demanding scientific workloads. With global infrastructure offering 99.99 percent availability, continuous innovation in high-performance computing, and deep expertise in genomics workflows, AWS provides the foundation for breakthrough research. Partner solutions like MemVerge’s MMBatch can enhance this foundation, in combination with comprehensive AWS offerings, security capabilities, and cost optimization at scale. As research organizations push the boundaries of scientific discovery, AWS offerings evolve to meet their growing needs, ensuring that technology never becomes a bottleneck to scientific progress.

Discover how your team can run faster, spend smarter, and hit every deadline—no matter how complex the science.

If you’re ready to dive deeper:

* [Watch our walkthrough demo to see MemVerge in action.](https://www.youtube.com/watch?v=kDBJyl5qF9g)  
* Try out our [MMEngine Workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/86acff36-22b7-4ac8-987e-2b21d493e60e/en-US) on AWS Workshop Studio to gain hands-on experience.  
* Talk to your MemVerge account team or visit [MemVerge](https://www.memverge.com/) to schedule a personalized workshop.

TAGS: [Amazon EC2](https://aws.amazon.com/blogs/publicsector/tag/amazon-ec2/), [AWS Batch](https://aws.amazon.com/blogs/publicsector/tag/aws-batch/), [AWS Partner Network](https://aws.amazon.com/blogs/publicsector/tag/aws-partner-network/), [AWS Public Sector](https://aws.amazon.com/blogs/publicsector/tag/aws-public-sector/), [AWS Public Sector Partners](https://aws.amazon.com/blogs/publicsector/tag/aws-public-sector-partners/), [life sciences](https://aws.amazon.com/blogs/publicsector/tag/life-sciences/), [nonprofit](https://aws.amazon.com/blogs/publicsector/tag/nonprofit/), [technical how-to](https://aws.amazon.com/blogs/publicsector/tag/technical-how-to/)

![Gabriela Karina Paulus, PhD](/images/image2_03.jpg)

### Gabriela Karina Paulus, PhD

Gabriela is a solutions architect at AWS, working within the NPO Sector and focusing on Research customers. She holds a PhD in Molecular Biology and Bioinformatics, a MSc. in Pharmacology, and a BSc. in Biotechnology. Based in the NYC Metropolitan Area, Gabriela is not only a solutions architect, but is also recognized as a genomics expert within AWS. She combines her scientific background with cloud computing expertise to drive innovation and to help scientists and research institutes perform their best work. Gabriela's expertise in cloud technology is complemented by her extensive experience in research, with her previous peer-reviewed publication contributing valuable insights.

![Christian Kniep](/images/image3_03.png)

### Christian Kniep

Christian’s passion is to improve the ease of use of those stubborn machines we call IT equipment. He started his career as an HPC Systems Engineer in the automotive industry and concluded the System admin job as the Infiniband lead of a crash-test cluster with thousands of nodes in the early 2010s. Afterwards he moved to Paris to work on HPC interconnects. Once Docker entered the picture in 2014, Christian moved back to Berlin and started working in (emerging) containerized environment at a Berlin-based Startup, Playstation Now, and Docker Inc. In 2019, Christian became an 'EC2 spot specialist SA' at AWS and became the first senior developer advocate within the HPC product team. In 2022, Christian left AWS to once more pursue his passion project to push container usage in HPC. He worked for the Max Planck Institute for Human Development and joined MemVerge as principal HPC architect to foster the integration of application checkpoints and cloudy HPC stacks.
