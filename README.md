# The Open Guide to Amazon Web Services

* [Why an Open Guide?](#why-an-open-guide)
* [Scope](#scope)
* [General Information](#general-information)
* [Managing AWS](#managing-aws)
* [Managing Servers](#managing-servers)
* [Billing and Cost Management](#billing-and-cost-management)
* [AWS Security and IAM](#aws-security-and-iam)
* [S3](#s3)
* [EC2](#ec2)
* [AMIs](#amis)
* [EBS](#ebs)
* [ELBs](#elbs)
* [Elastic IPs](#elastic-ips)
* [Glacier](#glacier)
* [RDS](#rds)
* [DynamoDB](#dynamodb)
* [ECS](#ecs)
* [Lambda](#lambda)
* [Route 53](#route-53)
* [CloudFormation](#cloudformation)
* [VPCs, Network Security, and Security Groups](#vpcs-network-security-and-security-groups)
* [CloudFront](#cloudfront)
* [DirectConnect](#directconnect)
* [High Availability](#high-availability)
* [Redshift](#redshift)
* [EMR](#emr)
* [Further Reading](#further-reading)
* [Disclaimer](#disclaimer)
* [License](#license)

## Why an Open Guide?

A lot of information on AWS is already written. Most people learn AWS by reading a blog or a “[getting started guide](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)” and referring to the standard AWS references. Nonetheless, trustworthy and practical information and recommendations aren’t easy to come by. [AWS’s own documentation](https://aws.amazon.com/documentation/) is a great resource but no one reads it all, and it doesn’t include anything but official facts, so omits experiences of engineers. The information in blogs or [Stack Overflow](http://stackoverflow.com/questions/tagged/amazon-web-services) is also not consistently up to date.

This guide aims to be a useful, living reference that consolidates links, tips, gotchas and best practices.
It arose from discussion and editing over beers by [several engineers](AUTHORS.md) who have used AWS extensively.
Please read the [**license**](#license) and [**disclaimer**](#disclaimer).

### Please help

**July 2016: This is an early in-progress draft!**
It’s our first attempt at assembling this information, so is certain to have omissions and errors.
[**Please contribute**](CONTRIBUTING.md) by filing issues or PRs to comment, expand, correct, or otherwise improve it.
This guide *open to contributions*, so unlike a blog, it can keep improving.
Like any open source effort, we combine efforts but also review ensure high quality.


## Scope

* Currently, this guide covers selected “core” services, such as EC2, S3, ELBs, EBS, and IAM, and partial details and tips around other services. We expect it to expand.
* It is not a tutorial, but rather a collection of information you can read and return to. It is for both beginners and the experienced.
* The goal of this guide is to be:
    * **Brief**: Keep it dense and use links
    * **Practical**: Basic facts, concrete facts, details, advice, gotchas, and “folk knowledge”
    * **Current**: We can keep updating it, and anyone can contribute improvements
    * **Thoughtful**: The goal is to be helpful rather than present dry facts. Thoughtful opinion with rationale is welcome. Suggestions, notes, and opinions based on real experience can be extremely valuable. (We believe this is both possible with a guide of this format, unlike in some [other venues](http://meta.stackexchange.com/questions/201994/is-there-a-place-to-ask-opinion-based-questions).)
* This guide is not sponsored by AWS or AWS-affiliated vendors. It is written by and for engineers who use AWS.
* Legend:
    * 🔹 Important or often overlooked tip
    * ❗ Gotcha or warning (where risks or time or resource costs are significant)
    * 🔸 Limitation or quirk (where it’s not quite so bad)
    * 🐥 Relatively new or immature services
    * ⏱ Performance discussions
    * ⛓ Lock-in (decisions that are likely to tie you to AWS in a new or significant way)
    * 🚪 Alternative non-AWS options
    * 💸 Cost issues and discussion
    * 🕍 A mild warning attached to “full solution” or opinionated frameworks that may take significant time to understand and/or might not fit your needs exactly; the opposite of a point solution (the cathedral is a nod to [Raymond’s metaphor](https://en.wikipedia.org/wiki/The_Cathedral_and_the_Bazaar))
    * 🚧 Areas where correction or improvement are needed (possibly with link to an issue — do help)


## General Information

### When to Use AWS

* [AWS](https://en.wikipedia.org/wiki/Amazon_Web_Services) is the dominant public cloud computing provider.
    * In general, “[cloud computing](https://en.wikipedia.org/wiki/Cloud_computing)” can refer to one of three types of cloud: “public,” “private,” and “hybrid.” AWS is a public cloud provider, since anyone can use it. Private clouds are within a single (usually large) organization. Many companies use a hybrid of private and public clouds.
    * The core features of AWS are [infrastructure-as-a-service](https://en.wikipedia.org/wiki/Cloud_computing#Infrastructure_as_a_service_.28IaaS.29) (IaaS) — that is, virtual machines and supporting infrastructure. Other cloud service models include [platform-as-a-service](https://en.wikipedia.org/wiki/Cloud_computing#Platform_as_a_service_.28PaaS.29) (PaaS), which typically are more fully managed services that deploy customers’ applications, or [software-as-a-service](https://en.wikipedia.org/wiki/Cloud_computing#Software_as_a_service_.28SaaS.29) (SaaS), which are cloud-based applications. AWS does offer a few products that fit into these other models, too.
    * In business terms, with infrastructure-as-a-service you have a variable cost model — it is  [OpEx, not CapEx](http://www.investopedia.com/ask/answers/020915/what-difference-between-capex-and-opex.asp) (though some [pre-purchased contracts](https://aws.amazon.com/ec2/purchasing-options/reserved-instances/) are still CapEx).
* **Main reasons to use AWS**:
    * If your company is building systems or products that may need to scale
    * and you have technical know-how
    * and you want the most flexible tools
    * and you’re not significantly tied into different infrastructure already
    * and you don’t have internal, regulatory, or compliance reasons you can’t use a public cloud-based solution
    * and you’re not on a Microsoft-first tech stack
    * and you don’t have a specific reason to use Google Cloud
    * and you can afford, manage, or negotiate its somewhat higher costs
    * ... then AWS is likely a good option for your company.
* Each of those reasons above might point to situations where other services are preferable. In practice, many, if not most, tech startups as well as a number of modern large companies fit those criteria. (Many large enterprises are partly migrating internal infrastructure to Azure, Google Cloud, and AWS.)
* 🚪**AWS vs. IaaS alternatives**: While AWS is the dominant IaaS provider (31% market share in [this 2016 estimate](https://www.srgresearch.com/articles/aws-remains-dominant-despite-microsoft-and-google-growth-surges)), there is significant of competition and alternatives that are better suited to some companies:
    * The most significant direct competitor is [**Google Cloud**](https://cloud.google.com/). It arrived later to market than AWS, but has vast resources and is now used widely by many companies, including a few large ones. It is gaining market share. Not all AWS services have similar or analogous services in Google Cloud. And vice versa: In particular Google offers some more advanced machine learning-based services like the [Vision API](https://cloud.google.com/vision/). It’s not common to switch once you’re up and running, but it does happen: [Spotify migrated](http://www.wsj.com/articles/google-cloud-lures-amazon-web-services-customer-spotify-1456270951) from AWS to Google Cloud. There is more discussion [on Quora](https://www.quora.com/What-are-the-reasons-to-choose-AWS-over-Google-Cloud-or-vice-versa-for-a-high-traffic-web-application) about relative benefits.
    * [**Microsoft Azure**](https://azure.microsoft.com/en) is the de facto choice for companies and teams that are focused on a Microsoft stack.
    * In China, AWS’ footprint is relatively small. The market is dominated by Alibaba’s [Aliyun](https://intl.aliyun.com/).
    * Companies at (very) large scale may want to reduce costs by managing their own infrastructure. For example, [Dropbox migrated](https://news.ycombinator.com/item?id=11282948) to their own infrastructure.
    * Other cloud providers such as [Digital Ocean](https://www.digitalocean.com/) offer similar services, sometimes with greater ease of use, more personalized support, or lower cost. However, none of these match the breadth of products, mind-share, and market domination AWS now enjoys.
    * Traditional managed hosting providers such as [Rackspace](https://www.rackspace.com/) offer cloud solutions as well.
* 🚪**AWS vs. PaaS**: If your goal is just to put up a single service that does something relatively simple, and you’re trying to minimize time managing operations engineering, consider a [platform-as-a-service](https://en.wikipedia.org/wiki/Platform_as_a_service) such as [Heroku](https://www.heroku.com/) The AWS approach to PaaS, Elastic Beanstalk, is arguably more complex, especially for simple use cases.
* 🚪**AWS vs. web hosting**: If your main goal is to host a website or blog, and you don’t expect to be building an app or more complex service, you may wish consider one of the myriad of [web hosting services](https://www.google.com/search?q=web+hosting).
* 🚪**AWS vs. managed hosting**: Traditionally, many companies pay [managed hosting](https://en.wikipedia.org/wiki/Dedicated_hosting_service) providers to maintain physical servers for them, then build and deploy their software on top of the rented hardware. This makes sense for businesses who want direct control over hardware, due to legacy, performance, or special compliance constraints, but is usually considered old fashioned or unnecessary by many developer-centric startups and younger tech companies.
* **Complexity**: AWS will let you build and scale systems to the size of the largest companies, but the complexity of the services when used at scale requires significant depth of knowledge and experience. Even very simple use cases often require more knowledge to do “right” in AWS than in a simpler environment like Heroku or Digital Ocean. (This guide may help!)
* **Geographic locations**: AWS has data centers in [about 10 geographic locations](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-available-regions) (known as **regions**) in Europe, Asia, and North and South America. If your infrastructure needs to be in close physical proximity to another service for latency or throughput reasons (for example, latency to an ad exchange), viability of AWS will depend on the location.
* ⛓**Lock-in:** As you use AWS, it’s important to be aware when you are depending on AWS services that do not have equivalents elsewhere. Basic services like virtual servers in EC2 are usually easy to migrate to other vendors, but the more services you use, the more lock-in you have to AWS, and the more difficult it will be to change to other providers in the future. It is quite common to mix and match services from different vendors (such as using S3 for storage but a different vendor for serving) and, in larger enterprises, to hybridize between private cloud or on-premises servers and AWS.
* **Major customers**: Who uses AWS and Google Cloud?
    * AWS’s [list of customers ](https://aws.amazon.com/solutions/case-studies/netflix/)includes a large numbers of mainstream sites, such as Netflix, Pinterest, Spotify, Airbnb, and Yelp.
    * Google Cloud’s [list of customers](https://cloud.google.com/customers/) is large as well, and includes a few mainstream sites, such as [Snapchat](http://www.businessinsider.com/snapchat-is-built-on-googles-cloud-2014-1), Best Buy, Domino’s, and Sony Music.

### Which services to use

* AWS offers a *lot* of different services — [about fifty](https://aws.amazon.com/products/) at last count.
* Most customers use a few services heavily, a few services lightly, and the rest not at all. What services you’ll use depends on your use cases. Choices  differ substantially from company to company.
* Just because AWS has a service that sounds promising, it doesn’t mean you should use it. Some services are very narrow in use case, not mature, are overly opinionated, or have limitations, so very few people use them. More on this next.
* Many customers combine AWS with other non-AWS services. For example, legacy systems or secure data might be in a managed hosting provider, while other systems are AWS. Or a company might only use S3 with another provider doing everything else. However small startups or projects starting fresh will typically stick to AWS or Google Cloud only.
* **Must-know infrastructure**: Most typical small to medium-size users will focus on the following services first. If you manage use of AWS systems, you likely need to know at least a little about all of these. (Even if you don’t use them, you should learn enough to make that choice intelligently.)
    * [IAM](https://aws.amazon.com/iam/): User accounts and identities (you need to think about accounts early on!)
    * [EC2](https://aws.amazon.com/ec2/): Virtual servers and associated components, including:
        * [AMIs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html): Machine Images
        * [ELBs](https://aws.amazon.com/elasticloadbalancing/): Load balancing
        * [Autoscaling](https://aws.amazon.com/autoscaling/): Capacity scaling (adding and removing servers based on load)
        * [EBS](https://aws.amazon.com/ebs/): Network-attached disks
        * [Elastic IPs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html): Assigned IP addresses
    * [S3](https://aws.amazon.com/s3/): Storage of files
    * [Route 53](https://aws.amazon.com/route53/): DNS and domain registration
    * [VPC](https://aws.amazon.com/vpc/): Virtual networking, network security, and co-location; you automatically use
    * [CloudFront](https://aws.amazon.com/cloudfront/): CDN for hosting content
    * [CloudWatch](https://aws.amazon.com/cloudwatch/): Alerts, paging, monitoring
* **Managed services**: Existing software solutions you could run on your own, but with managed deployment:
    * [RDS](https://aws.amazon.com/rds/): Managed relational databases (managed MySQL, Postgres, and Amazon’s own Aurora database)
    * [EMR](https://aws.amazon.com/elasticmapreduce/): Managed Hadoop
    * [Elasticsearch](https://aws.amazon.com/elasticsearch-service/): Managed Elasticsearch
    * [ElastiCache](https://aws.amazon.com/elasticache/): Managed Redis and Memcached
* **Optional but important infrastructure**: These are key and useful infrastructure are less widely known used. You may have legitimate reasons to prefer alternatives, so evaluate with care you to be sure they fit your needs:
    * [Lambda](https://aws.amazon.com/lambda/): Running small, fully managed tasks “serverless”
    * [CloudTrail](https://aws.amazon.com/cloudtrail/): AWS API logging and audit (often neglected but important)
    * 🕍[CloudFormation](https://aws.amazon.com/cloudformation/): Templatized configuration of collections of AWS resources
    * 🕍[Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/): Fully managed (PaaS) deployment of packaged Java, .NET, PHP, Node.js, Python, Ruby, Go, and Docker applications
    * 🐥[EFS](https://aws.amazon.com/efs/): Network filesystem
    * 🕍[ECS](https://aws.amazon.com/ecs/): Docker container/cluster management. Note Docker can be used directly, without ECS.
    * [ECR](https://aws.amazon.com/ecr/): Hosted private Docker registry.
    * 🐥[Config](https://aws.amazon.com/config/): AWS configuration inventory, history, change notifications
* **Special-purpose infrastructure**: These services are focused on specific use cases and should be evaluated if they apply to your situation:
    * [Glacier](https://aws.amazon.com/glacier/): Slow and cheap alternative to S3
    * [Kinesis](https://aws.amazon.com/kinesis/): Streaming (distributed log) service
    * [SQS](https://aws.amazon.com/sqs/): Message queueing service
    * [Redshift](https://aws.amazon.com/redshift/): Data warehouse
    * 🐥[QuickSight](https://aws.amazon.com/quicksight/): Business intelligence service
    * [SES](https://aws.amazon.com/ses/): Send and receive e-mail for marketing or transactions
    * [DynamoDB](https://aws.amazon.com/dynamodb/): Low-latency NoSQL key-value store
    * [API Gateway](https://aws.amazon.com/api-gateway/): Proxy, manage, and secure API calls
    * [WAF](https://aws.amazon.com/waf/): Web firewall for CloudFront to deflect attacks
    * [KMS](https://aws.amazon.com/kms/): Store and manage encryption keys securely
    * [Inspector](https://aws.amazon.com/inspector/): Security audit
    * [Trusted Advisor](https://aws.amazon.com/premiumsupport/trustedadvisor/): Automated tips on reducing cost or making improvements
* ⛓🕍**Compound services**: These are similarly specific, but are full-blown services that tackle complex problems and may tie you in. Usefulness depends on your requirements. If you have large or significant need, you may have these already managed by in-house systems and engineering teams:
    * [Machine Learning](https://aws.amazon.com/machine-learning/): Machine learning model training and classification
    * [Data Pipeline](https://aws.amazon.com/datapipeline/): Managed ETL service
    * [SWF](https://aws.amazon.com/swf/): Managed background job workflow
    * [Lumberyard](https://aws.amazon.com/lumberyard/): 3D game engine
* **Mobile/app development**:
    * [SNS](https://aws.amazon.com/sns/): Manage app push notifications and other end-user notifications
    * [Cognito](https://aws.amazon.com/cognito/): User authentication via Facebook, Twitter, etc.
    * [Device Farm](https://aws.amazon.com/device-farm/): Cloud-based device testing
    * [Mobile Analytics](https://aws.amazon.com/mobileanalytics/): Analytics solution for app usage
    * 🕍[Mobile Hub](https://aws.amazon.com/mobile/): Comprehensive, managed mobile app framework
* **Enterprise services**: These are relevant if you have significant corporate cloud-based or hybrid needs. Many smaller companies and startups use other solutions, like Google Apps or Box. Larger companies may also have their own non-AWS IT solutions.
    * [AppStream](https://aws.amazon.com/appstream/): Windows apps in the cloud, with access from many devices
    * [Workspaces](https://aws.amazon.com/workspaces/): Windows desktop in the cloud, with access from many devices
    * [WorkDocs](https://aws.amazon.com/workdocs/) (formerly Zocalo): Enterprise document sharing
    * [WorkMail](https://aws.amazon.com/workmail/): Enterprise managed e-mail and calendaring service
    * [Directory Service](https://aws.amazon.com/directoryservice/): Microsoft Active Directory in the cloud
    * [Direct Connect](https://aws.amazon.com/directconnect/): Dedicated network connection between office or data center and AWS
    * [Storage Gateway](https://aws.amazon.com/storagegateway/): Bridge between on-premises IT and cloud storage
    * [Service Catalog](https://aws.amazon.com/servicecatalog/): IT service approval and compliance
* **Probably-don't-need-to-know services**: Bottom line, our informal polling indicates these services are just not broadly used — and often for good reasons:
    * [Snowball](https://aws.amazon.com/importexport/): If you want to ship petabytes of data into or out of Amazon using a physical appliance, read on.
    * [CodeCommit](https://aws.amazon.com/codecommit/): Git service. You’re probably already using GitHub or your own solution ([Stackshare](http://stackshare.io/stackups/github-vs-bitbucket-vs-aws-codecommit) has informal stats).
    * 🕍[CodePipeline](https://aws.amazon.com/codepipeline/): Continuous integration. You likely have another solution already.
    * 🕍[CodeDeploy](https://aws.amazon.com/codedeploy/): Deployment of code to EC2 servers. Again, you likely have another solution.
    * 🕍[OpsWorks](https://aws.amazon.com/opsworks/): Management of your deployments using Chef. While Chef is popular, it seems few people use OpsWorks, since it involves going in on a whole different code deployment framework.
* [AWS in Plain English](https://www.expeditedssl.com/aws-in-plain-english) offers more friendly explanation of what all the other different services are.

### Service matrix

Many services within AWS can at least be compared with Google Cloud offerings or with internal Google services. And often times you could assemble the same thing yourself with open source software. This table is an effort at listing these rough correspondences. (Remember that this table is imperfect as in almost every case there are subtle differences of features!)

| Service | AWS | Google Cloud | Google Internal | Microsoft | Other providers | Open source “build your own” |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Virtual server | EC2 | Compute Engine (GCE) |  |  | DigitalOcean | OpenStack |
| PaaS | Elastic Beanstalk | App Engine | App Engine |  | Heroku | Meteor, AppScale |
| Serverless, microservices | Lambda | Functions |  |  |  | |
| Container, cluster manager | ECS | Container Engine/Kubernetes | Borg or Omega |  |  | Kubernetes, Mesos/Aurora |
| File storage | S3 | Cloud Storage | GFS |  |  | Swift, HDFS |
| Block storage | EBS | Persistent Disk |  |  |  | NFS |
| SQL datastore | RDS | Cloud SQL |  |  |  | MySQL, PostgreSQL |
| Sharded RDBMS |  | Cloud SQL | F1, Spanner |  |  | Crate.io |
| Bigtable |  | Cloud Bigtable | Bigtable |  |  | CockroachDB |
| Key-value store, column store | DynamoDB | Cloud Datastore | Megastore |  |  | Cassandra, CouchDB, RethinkDB, Redis |
| Memory cache | ElastiCache | App Engine Memcache |  |  |  | Memcached, Redis |
| Search | CloudSearch |  |  |  | Algolia, QBox | Elasticsearch, Solr |
| Data warehouse | Redshift | BigQuery |  |  | Oracle, IBM, SAP, HP, many others | Greenplum |
| Business intelligence | QuickSight |  |  |  | Tableau |
| Lock manager | [DynamoDB (weak)](https://gist.github.com/ryandotsmith/c95fd21fab91b0823328) |  | Chubby |  |  | ZooKeeper, Etcd, Consul |
| Message broker | SQS | Pub/Sub | PubSub2 |  |  | RabbitMQ, Kafka, 0MQ |
| Streaming, distributed log | Kinesis | Dataflow | PubSub2 | Event Hubs |  | Kafka Streams, Apex, Flink, Spark Streaming, Storm |
| MapReduce | EMR | Dataproc | MapReduce |  | Qubole | Hadoop |
| Monitoring | CloudWatch | Monitoring | Borgmon |  |  | Prometheus(?) |
| Metric management |  |  | Borgmon, TSDB |  |  | Graphite, InfluxDB, OpenTSDB, Grafana, Riemann, Prometheus |
| CDN | CloudFront |  |  | Azure CDN |  | Apache Traffic Server |
| Load balancer | ELB | Load Balancing | GFE |  |  | nginx, HAProxy, Apache Traffic Server |
| DNS | Route53 | DNS |  |  |  | bind |
| Email | SES |  |  |  | Sendgrid, Mandrill, Postmark |
| Git hosting | CodeCommit |  |  |  | GitHub, BitBucket | GitLab |
| User authentication | Cognito |  |  |  |  | oauth.io |
| Mobile app analytics | Mobile Analytics |  |  |  |  | Mixpanel |


Selected resources with more detail on this chart:

* Google internal: [MapReduce](http://research.google.com/archive/mapreduce.html), [Bigtable](http://research.google.com/archive/bigtable.html), [Spanner](http://research.google.com/archive/spanner.html), [F1 vs Spanner](http://highscalability.com/blog/2013/10/8/f1-and-spanner-holistically-compared.html), [Bigtable vs Megastore](http://perspectives.mvdirona.com/2008/07/google-megastore/)

### Maturity and Releases

It’s important to know the maturity of each product. Here is a mostly complete list of first release date, with links to the [release notes](https://aws.amazon.com/releasenotes/). Most recently released services are first. Not all services are available in all regions; see [this table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/).

| Service | Original release | Availability |
| --- | --- | --- |
| [Database Migration Service](https://aws.amazon.com/releasenotes/AWS-Database-Migration-Service?browse=1) | 2016-03 | General |
| [WAF](https://aws.amazon.com/releasenotes/AWS-WAF?browse=1) | 2015-10 | General |
| [Data Pipeline](https://aws.amazon.com/releasenotes/AWS-Data-Pipeline?browse=1) | 2015-10 | General |
| [Elasticsearch](https://aws.amazon.com/releasenotes/Amazon-Elasticsearch-Service?browse=1) | 2015-10 | General |
| [Service Catalog](https://aws.amazon.com/releasenotes/AWS-Service-Catalog?browse=1) | 2015-07 | General |
| [CodePipeline](https://aws.amazon.com/releasenotes/AWS-CodePipeline?browse=1) | 2015-07 | General |
| [CodeCommit](https://aws.amazon.com/releasenotes/AWS-CodeCommit?browse=1) | 2015-07 | General |
| [API Gateway](https://aws.amazon.com/releasenotes/Amazon-API-Gateway?browse=1) | 2015-07 | General |
| [Config](https://aws.amazon.com/releasenotes/AWS-Config?browse=1) | 2015-06 | General |
| [EFS](https://aws.amazon.com/releasenotes/Amazon-EFS?browse=1) | 2015-05 | Preview |
| [Machine Learning](https://aws.amazon.com/releasenotes/AmazonML?browse=1) | 2015-04 | General |
| [Lambda](https://aws.amazon.com/releasenotes/AWS-Lambda?browse=1) | 2014-11 | General |
| [ECS](https://aws.amazon.com/ecs/release-notes/) | 2014-11 | General |
| [KMS](https://aws.amazon.com/releasenotes/AWS-KMS?browse=1) | 2014-11 | General |
| [CodeDeploy](https://aws.amazon.com/releasenotes/AWS-CodeDeploy?browse=1) | 2014-11 | General |
| [Kinesis](https://aws.amazon.com/releasenotes/Amazon-Kinesis?browse=1) | 2013-12 | General |
| [CloudTrail](https://aws.amazon.com/releasenotes/AWS-CloudTrail?browse=1) | 2013-11 | General |
| [AppStream](https://aws.amazon.com/releasenotes/Amazon-AppStream?browse=1) | 2013-11 | Preview |
| [CloudHSM](https://aws.amazon.com/releasenotes/AWS-CloudHSM?browse=1) | 2013-03 | General |
| [Silk](https://aws.amazon.com/releasenotes/Amazon-Silk?browse=1) | 2013-03 | Obsolete? |
| [OpsWorks](https://aws.amazon.com/releasenotes/AWS-OpsWorks?browse=1) | 2013-02 | General |
| [Redshift](https://aws.amazon.com/releasenotes/Amazon-Redshift?browse=1) | 2013-02 | General |
| [Elastic Transcoder](https://aws.amazon.com/releasenotes/Amazon-Elastic-Transcoder?browse=1) | 2013-01 | General |
| [Glacier](https://aws.amazon.com/releasenotes/Amazon-Glacier?browse=1) | 2012-08 | General |
| [CloudSearch](https://aws.amazon.com/releasenotes/Amazon-CloudSearch?browse=1) | 2012-04 | General |
| [SWF](https://aws.amazon.com/releasenotes/Amazon-SWF?browse=1) | 2012-02 | General |
| [Storage Gateway](https://aws.amazon.com/releasenotes/AWS-Storage-Gateway?browse=1) | 2012-01 | General |
| [DynamoDB](https://aws.amazon.com/releasenotes/Amazon-DynamoDB?browse=1) | 2012-01 | General |
| [DirectConnect](https://aws.amazon.com/releasenotes/AWS-Direct-Connect?browse=1) | 2011-08 | General |
| [ElastiCache](https://aws.amazon.com/releasenotes/Amazon-ElastiCache?browse=1) | 2011-08 | General |
| [CloudFormation](https://aws.amazon.com/releasenotes/AWS-CloudFormation?browse=1) | 2011-04 | General |
| [SES](https://aws.amazon.com/releasenotes/Amazon-SES?browse=1) | 2011-01 | General |
| [Elastic Beanstalk](https://aws.amazon.com/releasenotes/AWS-Elastic-Beanstalk?browse=1) | 2010-12 | General |
| [Route 53](https://aws.amazon.com/releasenotes/Amazon-Route-53?browse=1) | 2010-10 | General |
| [IAM](https://aws.amazon.com/releasenotes/AWS-Identity-and-Access-Management?browse=1) | 2010-09 | General |
| [SNS](https://aws.amazon.com/releasenotes/Amazon-SNS?browse=1) | 2010-04 | General |
| [EMR](https://aws.amazon.com/releasenotes/Elastic-MapReduce?browse=1) | 2010-04 | General |
| [RDS](https://aws.amazon.com/releasenotes/Amazon-RDS?browse=1) | 2009-12 | General |
| [VPC](https://aws.amazon.com/releasenotes/Amazon-VPC?browse=1) | 2009-08 | General |
| [Snowball](https://aws.amazon.com/releasenotes/AWS-ImportExport?browse=1) | 2009-05 | General |
| [CloudWatch](https://aws.amazon.com/releasenotes/CloudWatch?browse=1) | 2009-05 | General |
| [CloudFront](https://aws.amazon.com/releasenotes/CloudFront?browse=1) | 2008-11 | General |
| [Fulfillment Web Service](https://aws.amazon.com/releasenotes/Amazon-FWS?browse=1) | 2008-03 | Obsolete? |
| [SimpleDB](https://aws.amazon.com/releasenotes/Amazon-SimpleDB?browse=1) | 2007-12 | Obsolete |
| [DevPay](https://aws.amazon.com/releasenotes/DevPay?browse=1) | 2007-12 | General |
| [Flexible Payments Service](https://aws.amazon.com/releasenotes/Amazon-FPS?browse=1) | 2007-08 | Retired |
| [EC2](https://aws.amazon.com/releasenotes/Amazon-EC2?browse=1) | 2006-08 | General |
| [SQS](https://aws.amazon.com/releasenotes/Amazon-SQS?browse=1) | 2006-07 | General |
| [S3](https://aws.amazon.com/releasenotes/Amazon-S3?browse=1) | 2006-03 | General |


### Compliance

* Many applications have strict requirements around reliability, security, or data privacy. The [AWS Compliance](https://aws.amazon.com/compliance/) page has details about AWS’s certifications, which include **PCI DSS Level 1**, **SOC 3**, and **ISO 9001**.
* Security in the cloud is a complex topic, based on a [shared responsibility model](https://aws.amazon.com/compliance/shared-responsibility-model/), where some elements of compliance are provided by AWS, and some are provided by your company.
* Several third-party vendors offer assistance with compliance, security, and auditing on AWS. If you have substantial needs in these areas, assistance is a good idea.
* In **China**, AWS services [are generally accessible](https://en.greatfire.org/aws.amazon.com), though there are at times breakages in service

### Getting Help and Support

* **Forums**: For many problems, it’s worth searching or asking for help in the [discussion forums](https://forums.aws.amazon.com/index.jspa) to see if it’s a known issue.
* **Premium support**: AWS offers several levels of [premium support](https://aws.amazon.com/premiumsupport/).
    * Any small company should probably pay for the cheap “Developer” support as it’s a flat $49/month and it lets you file support tickets with 12 to 24 hour turnaround time.
    * The higher-level support services are quite expensive — and increase your bill by at least 10%. Many large and effective companies never pay for this level of support. They are usually more helpful for midsize or larger companies needing rapid turnaround on deeper or more perplexing problems.
    * Keep in mind, a flexible architecture can reduce need for support. You shouldn’t be relying on AWS to solve your problems often. For example, if you can easily re-provision a new server, it may not be urgent to solve a rare kernel-level issue unique to one EC2 instance. If your EBS volumes have recent snapshots, you may be able to restore a volume before support can rectify the issue with the old volume. If your services have an issue in one availability zone, you should in any case be able to rely on a redundant zone or migrate services to another zone.
    * Larger customers also get access to AWS Enterprise support, with dedicated technical account managers (TAMs) and shorter response time SLAs.
    * There is definitely some controversy about how useful the paid support is. The support staff don’t always seem to have the information and authority to solve the problems that are brought to their attention. Often your ability to have a problem solved may depend on your relationship with your account rep.
* **Account manager**: If you are at significant levels of spend (thousands of US dollars plus per month), you may be assigned (or may wish to ask for) a dedicated account manager.
    * These are a great resource, even if you’re not paying for premium support. Build a good relationship with them and make use of them, for questions, problems, and guidance.
    * Assign a single point of contact on your company’s side, to avoid confusing or overwhelming them.
* **Contact**: The main web contact point for AWS is [here](https://aws.amazon.com/contact-us/). Many technical requests can be made via these channels.
* **Consulting**: For more hands-on assistance, AWS maintains a list of [consulting partners](https://aws.amazon.com/partners/consulting/). These won’t be cheap but depending on your needs, may save you costs long term by helping you set up your architecture more effectively, or offering specific expertise, e.g. security.

### Restrictions and Other Notes

* 🔸Lots of resources in Amazon have [**limits**](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) on them. This is actually helpful, so you don’t incur large costs accidentally. You have to request that quotas be increased by opening support tickets. Some limits are easy to raise, and some are not. (Some of these are noted in sections below.)
* 🔸[**AWS terms of service**](https://aws.amazon.com/service-terms/) are extensive. Much is expected boilerplate, but it does contain important notes and restrictions on each service. In particular, there are restrictions against using many AWS services in **safety-critical systems**. (Those appreciative of legal humor may wish to review clause 57.10.)

### Related Topics

* [OpenStack](https://www.openstack.org/) is a private cloud alternative to AWS used by large companies that wish to avoid public cloud offerings.


## Managing AWS

### Managing Infrastructure State and Change

A great challenge in using AWS to build complex systems (and with DevOps in general) is to manage infrastructure state effectively over time. In general, this boils down to three broad goals for the state of your infrastructure:

* *Visibility*: Do you know the state of your infrastructure (what services you are using, and exactly how)? Do you also know when you — and anyone on your team — make changes? Can you detect misconfigurations, problems, and incidents with your service?
* *Automation*: Can you reconfigure your infrastructure to reproduce past configurations or scale up existing ones without a lot of extra manual work, or requiring knowledge that’s only in someone’s head? Can you respond to incidents easily or automatically?
* *Flexibility*: Can you improve your configurations and scale up in new ways without significant effort? Can you add more complexity using the same tools? Do you share, review, and improve your configurations within your team?

Much of what we discuss below is really about how to improve the answers to these questions.

There are several approaches to deploying infrastructure with AWS, from the console to complex automation tools, to third-party services, all of which attempt to help achieve visibility, automation, and flexibility.

### AWS Configuration Management

The first way most people experiment with AWS is via its web interface, the AWS Console. But using the Console is a highly manual process, and often works against automation or flexibility.

So if you’re not going to manage your AWS configurations manually, what should you do? Sadly, there are no simple, universal answers — each approach has pros and cons, and the approaches taken by different companies vary widely, and include directly using APIs (and building toolign on top yourself), using command-line tools, and using third-party tools and services.

### AWS Console

* The [AWS Console](https://aws.amazon.com/console/) lets you control much (but not all) functionality of AWS via a web interface.
* Ideally, you should only use the AWS Console in a few specific situations:
    * It’s great for read-only usage. If you’re trying to understand the state of your system, logging in and browsing it is very helpful.
    * It is also reasonably workable for very small systems and teams (for example, one engineer setting up one server that doesn’t change often).
    * It can be useful for operations you’re only going to do rarely, like less than once a month. In this case using the console can be the simplest approach.
* ❗However, if you’re likely to be making the same change multiple times, *avoid the console*. Favor some sort of automation, or at least have a path toward automation, as discussed next. Not only does using the console preclude automation, which wastes time later, but it prevents documentation, clarity, and standardization around processes for yourself and your team.

### Command-Line tools

* The [**aws command-line interface**](https://aws.amazon.com/cli/) (CLI), used via the **aws** command, is the most basic way to save and automate AWS operations.
* Don’t underestimate its power. It also has the advantage of being well-maintained — it covers a large proportion of all AWS services, and is up to date.
* In general, whenever you can, prefer the command line to the AWS Console for performing operations.
* 🔹Even in absence of fancier tools, you can **write simple Bash scripts** that invoke *aws* with specific arguments, and check these into Git. This is a primitive but effective way to document operations you’ve performed. It improves automation, allows code review and sharing on a team, and gives others a starting point for future work.
* 🔹For use that is primarily interactive, and not scripted, consider instead using [**saws**](https://github.com/donnemartin/saws). It is easier to use, with auto-completion and a colorful UI, but still works on the command line. Another similar option is AWS’s own [**aws-shell**](https://github.com/awslabs/aws-shell).

### APIs and SDKs

* **SDKs** for using AWS APIs are available in most major languages, with [Go](https://github.com/aws/aws-sdk-go), [iOS](https://github.com/aws/aws-sdk-ios), [Java](https://github.com/aws/aws-sdk-java), [JavaScript](https://github.com/aws/aws-sdk-js), [Python](https://github.com/boto/boto3), [Ruby](https://github.com/aws/aws-sdk-ruby), and [PHP](https://github.com/aws/aws-sdk-php) being most heavily used. AWS maintains [a short list](http://docs.aws.amazon.com/AmazonCloudFront/latest/APIReference/AWSLibraries.html), but the [awesome-aws list](https://github.com/donnemartin/awesome-aws#sdks-and-samples) is the most comprehensive and current. Note [support for C++](https://github.com/donnemartin/awesome-aws#c-sdk) is [still new](https://aws.amazon.com/blogs/aws/introducing-the-aws-sdk-for-c/).

### Boto

* A good way to automate operations in a custom way is [**Boto3**](https://github.com/boto/boto3), also known as the [Amazon SDK for Python](http://aws.amazon.com/sdk-for-python/). [**Boto2**](https://github.com/boto/boto), the previous version of this library, has been in wide use for years, but now there is a newer version with official support from Amazon, so prefer Boto3 for new projects.
* If you find yourself writing a Bash script with more than one or two CLI commands, you’re probably doing it wrong. Stop, and consider writing a Boto script instead. This has the advantages that you can:

    * Check return codes easily so success of each step depends on success of past steps.
    * Grab interesting bits of data from responses, like instance ids or DNS names.
    * Add useful environment information (for example, tag your instances with git revisions, or inject the latest build identifier into your initialization script).
    * Here’s a [rough example](https://github.com/iodine/openfda-internal/blob/master/openfda/deploy/aws_util.py).

### Third-Party Tools and Services

* **Tools**: Some open source tools can help manage or monitor AWS resources, such as [Netflix Ice](https://github.com/Netflix/ice) or [Security Monkey](https://github.com/Netflix/security_monkey) or [Cloud Custodian](https://github.com/capitalone/cloud-custodian).
* **Third-party services**: Several companies offer services designed to help you gain insights into expenses or lower your AWS bill, such as [OpsClarity](http://http//www.opsclarity.com/), [Cloudability](https://www.cloudability.com/), [CloudHealth Technologies](https://www.cloudhealthtech.com/), and [ParkMyCloud](http://www.parkmycloud.com/).

### General Visibility

* [Tagging resources](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html) is a great practice, especially as organizations grow, to better understand your resource usage. For example, you can through automation or convention add tags:
    * For the org or developer that “owns” that resource
    * For the product that resource supports
    * To label lifecycles, such as temporary resources or one that should be deprovisioned in the future
    * To distinguish production-critical infrastructure (e.g. serving systems vs backend pipelines)
    * To distinguish resources with special security or compliance requirements


## Managing Servers

### AWS vs Server Configuration

This guide is about AWS, not DevOps or server configuration management in general. But before getting into AWS in detail, it’s worth noting that in addition to the configuration management for your AWS resources, there is the long-standing problem of configuration management for servers themselves.

### Philosophy

* Heroku’s [**Twelve-Factor App**](http://12factor.net/) principles list some established general best practices for deploying applications.
* **Pets vs cattle**: Treat servers [like cattle, not pets](https://blog.engineyard.com/2014/pets-vs-cattle). That is, design systems so infrastructure is disposable. It should be minimally worrisome if a server is unexpectedly destroyed.
* The concept of [**immutable infrastructure**](http://radar.oreilly.com/2015/06/an-introduction-to-immutable-infrastructure.html) is an extension of this idea.

### Server Configuration Management

* There is a [large set](https://en.wikipedia.org/wiki/Comparison_of_open-source_configuration_management_software) of open source tools for managing configuration of server instances.
* These are generally not dependent on any particular cloud infrastructure, and work with any variety of Linux (or in many cases, a variety of operating systems).
* Leading configuration management tools are [Puppet](https://github.com/puppetlabs/puppet), [Chef](https://github.com/chef/chef), [Ansible](https://github.com/ansible/ansible), and [Saltstack](https://github.com/saltstack/salt). These aren’t the focus of this guide, but we may mention them as they relate to AWS.

### Containers and AWS

* [Docker](http://blog.scottlowe.org/2014/03/11/a-quick-introduction-to-docker/) and the containerization trend are changing the way many servers and services are deployed in general.
* Containers are designed as a way to package up your application(s) and all of their dependencies in a known way.  When you build a container, you are including every library or binary your application needs, outside of the kernel. A big advantage of this approach is that it’s easy to test and validate a container locally without worrying about some difference between your computer and the servers you deploy on.
* A consequence of this is that you need fewer AMIs and boot scripts; for most deployments, the only boot script you need is a template that fetches an exported docker image and runs it.
* Companies that are embracing [microservice architectures](http://martinfowler.com/articles/microservices.html) will often turn to container-based deployments.
* AWS launched [ECS](https://aws.amazon.com/ecs/) as a service to manage clusters via Docker in late 2014, though many people still deploy Docker directly themselves. See the [ECS section](#ecs) for more details.


## Billing and Cost Management

* AWS offers a [**free tier**](https://aws.amazon.com/free/) of service, that allows very limited usage of resources at no cost. For example, a micro instance and small amount of storage is available for no charge. (If you have an old account but starting fresh, sign up for a new one to qualify for the free tier.) [AWS Activate](https://aws.amazon.com/activate/) extends this to tens of thousands of dollars of free credits to startups in [certain funds or accelerators](https://aws.amazon.com/activate/portfolio-detail/).
* You can set [**billing alerts**](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/free-tier-alarms.html) to be notified of unexpected costs, such as costs exceeding the free tier.
* AWS offers [Cost Explorer](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-explorer-what-is.html), a tool to get better visibility into costs.
* For significant visibility, however, also consider third-party services like the ones above.
    * Sometimes, the AWS billing console times out or is too slow to use. In such case, third-party tools (like [Ice](https://github.com/Netflix/ice) — see above) may be a better option.
* AWS’s [Trusted Advisor ](https://aws.amazon.com/premiumsupport/trustedadvisor/)is another service that can help with cost concerns.
* Don’t be shy about asking your account manager for guidance in reducing your bill. It’s their job to keep you happily using AWS.
* **Tagging for cost visibility**: As the infrastructure grows, a key part of managing costs is understanding where they lie. It’s strongly advisable to [tag resources](https://aws.amazon.com/blogs/aws/resource-groups-and-tagging/), and as complexity grows, group them effectively. If you [set up billing allocation appropriately](http://aws.amazon.com/blogs/aws/aws-cost-allocation/), you can then get visibility into expenses according to organization, product, individual engineer, or any other way that is helpful.
* If you need to do custom analysis of raw billing data or want to feed it to a third party cost analysis service, [enable](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/detailed-billing-reports.html#turnonreports) the [detailed billing report](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/detailed-billing-reports.html#detailed-billing-report) feature.
* Multiple Amazon accounts can be linked for billing purposes using the [Consolidated Billing](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/consolidated-billing.html) feature. Large enterprises may need complex billing structures depending on ownership and approval processes.


## AWS Security and IAM

We cover overall security first, since configuring user accounts is something you usually have to do early on when setting up your system.

* ❗A lot of first-time AWS users create one account and one set of credentials, and then use them for a while, sharing among engineers and others within a company. This is easy. But *don’t do this*.
* 🔹Use IAM to create individual user accounts and **use them from the beginning**. This is slightly more work, but not that much.
    * That way, you define different users, and groups with different levels of privilege (if you want, choose from Amazon’s default suggestions, of administrator, power user, etc.).
    * This allows credential revocation, which is critical in some situations. If an employee leaves, or a key is compromised, you can revoke credentials with little effort.
    * Organizing your IAM users and groups according to the [principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege) means the security of your system is much higher.
* **Multiple accounts:** Decide on whether you want to use multiple AWS accounts and [research](https://dab35129f0361dca3159-2fe04d8054667ffada6c4002813eccf0.ssl.cf1.rackcdn.com/downloads/pdfs/Rackspace%20Best%20Practices%20for%20AWS%20-%20Identity%20Managment%20-%20Billing%20-%20Auditing.pdf) how to organize access across them. Factors to consider:
    * Number of users
    * Importance of isolation
        * Resource Limits
        * Permission granularity
        * Security
        * API Limits
    * Regulatory issues
    * Workload
    * Size of infrastructure
    * Cost of multi-account “overhead”: Internal AWS service management tools may need to be custom built or adapted.
* ❗Enable [**multi-factor authentication (MFA)**](https://aws.amazon.com/iam/details/mfa/) on your account.
    * You should always use MFA, and the sooner the better — enabling it when you already have many users is extra work.
    * Unfortunately it can’t be enforced in software, so an administrative policy has to be established.
    * Most users can use the Google Authenticator app (on [iOS](https://itunes.apple.com/us/app/google-authenticator/id388497605) or [Android](https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2)) to support two-factor authentication. For the root account, consider a hardware fob.
* 🔹Consider creating separate AWS accounts for independent parts of your infrastructure if you expect a high rate of AWS API calls, since AWS [throttles calls](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/query-api-troubleshooting.html#api-request-rate) at the AWS account level.
* [**Key Management Service (KMS)**](https://aws.amazon.com/kms/) is likely one of your best and most secure options for storing keys, such as for [EBS](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html) and [S3 encryption](http://docs.aws.amazon.com/AmazonS3/latest/dev/serv-side-encryption.html). (⛓ At the cost of lock-in.)
* [**AWS WAF**](https://aws.amazon.com/waf) is a web application firewall to help you protect your applications for common attack patterns.


## S3

### Tips

* For most practical purposes, you can consider S3 capacity unlimited, both in total size of files and number of objects.
* The buckets use a global naming scheme , so if another AWS has already created a bucket under a name that you want to use you will need to pick a different name. A common practice is to use the company name acronym or abbreviation to prefix all bucket names (but please, don’t use this as a security measure).
* The number of objects in a bucket is essentially unlimited. Customers routinely have millions of objects.
* **Durability**: Durability of S3 is extremely high, since internally it keeps several replicas. If you don’t delete it by accident, you can count on S3 not losing your data. (AWS offers the seemingly improbable durability rate of [99.999999999%](https://aws.amazon.com/s3/faqs/#How_durable_is_Amazon_S3), but this is a mathematical calculation based on independent failure rates and levels of replication — not a true probability estimate. Either way, S3 has had [a very good record](https://www.quora.com/Has-Amazon-S3-ever-lost-data-permanently) of durability.) Note this is *much* higher durability than EBS! If durability is less important for your application, you can use [S3 Reduced Redundancy Storage](https://aws.amazon.com/s3/reduced-redundancy/), which lowers the cost per GB, as well as the redundancy.
* ⏱**Performance**: Data throughput is complex, both in terms of bandwidth and number of operations:
    * Throughput is of course highest from within AWS, and between EC2 instances and S3 buckets that are in the same region.
    * Throughput is extremely high when accessed in a distributed way, from many EC2 instances. It’s possible to read or write objects from S3 from thousands of instances at once.
    * However, throughput is very limited when accessed sequentially, from a single instance. Individual operations take many milliseconds, and bandwidth to and from instances is limited by instance type.
    * Therefore, to perform large numbers of operations, it’s necessary to use high levels of parallelization, both in terms of threads and EC2 instances.
    * For large objects you want to take advantage of the multi-part uploading capabilities (starting with minimum chunk sizes of 5 MB).
    * Also you can download chunks in parallel by exploiting the HTTP GET range-header capability.
    * Listing contents happens at 1000 responses per request, so for buckets with many millions of objects listings will take time.
    * 🔸 In addition, latency on operations is [highly dependent on prefix similarities among key names](http://docs.aws.amazon.com/AmazonS3/latest/dev/request-rate-perf-considerations.html). If you have need for high volumes of operations, it is essential to consider naming schemes with more randomness early in the key name (first 7 or 8 characters) in order to avoid “hot spots”.
    * 🔸 Note that sadly, the latter advice about random key names goes against having a consistent layout with common prefixes to manage data lifecycles in an automated way.
* 💸**S3 pricing** depends on [storage, requests, and transfer](http://docs.aws.amazon.com/AmazonS3/latest/dev/request-rate-perf-considerations.html).
    * For transfer, putting data into AWS is free, but you’ll pay on the way out. Transfer from S3 to EC2 in the *same region *is free. Transfer to other regions or the Internet in general is not free.
* **Command-line applications**: There are a few ways to use S3 from the command line:
    * Originally, [**s3cmd**](https://github.com/s3tools/s3cmd) was the best tool for the job. It’s still used heavily by many.
    * The regular [**aws**](https://aws.amazon.com/cli/) command-line interface now supports S3 well, and is useful for most situations.
    * [**s4cmd**](https://github.com/bloomreach/s4cmd) is a replacement, with greater emphasis on performance via multi-threading, which is helpful for large files and large sets of files, and also offers Unix-like globbing support.
* **GUI applications**: You may prefer a GUI, or wish to support GUI access for less technical users. Some options:
    * The [AWS Console](https://aws.amazon.com/console/) does offer a graphical way to use S3. Use caution telling non-technical people to use it, however, since without tight permissions, it offers access to many other AWS features.
    * [Transmit](https://panic.com/transmit/) is a good option on OS X.
* **S3 and CloudFront**: S3 is tightly integrated with the CloudFront CDN. See the CloudFront section for more information.
* **Static website hosting:**
    * S3 has a [static website hosting option](http://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html) that is simply a setting that enables configurable HTTP index and error pages and [HTTP redirect support](http://docs.aws.amazon.com/AmazonS3/latest/dev/how-to-page-redirect.html) to [public content](http://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteAccessPermissionsReqd.html) in S3. It’s a simple way to host static assets or a fully static website.
    * Consider using CloudFront in front of most or all assets:
        * Like any CDN, CloudFront improves performance significantly.
        * 🔸 SSL is only supported on the built-in amazonaws.com domain. S3 does support serving these sites through a [custom domain](http://docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-custom-domain-walkthrough.html), but [not over SSL on a custom domain](http://stackoverflow.com/questions/11201316/how-to-configure-ssl-for-amazon-s3-bucket).
        * 🔸 If you are including resources across domains, such as fonts inside CSS files, you may need to [configure CORS](https://docs.aws.amazon.com/AmazonS3/latest/dev/cors.html) for the bucket serving those resources.
        * Since pretty much everything is moving to SSL nowadays, and you likely want control over the domain, you probably want to set up CloudFront your own certificate in front of S3 (and to ignore the [AWS example on this](http://docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-custom-domain-walkthrough.html) as it is non-SSL only).
        * That said, if you do, you’ll need to think through invalidation or updates on CloudFront. You may wish to [include versions or hashes in filenames](https://abhishek-tiwari.com/post/CloudFront-design-patterns-and-best-practices) so invalidation is not necessary.
* **Permissions:**
    * 🔸It’s important to manage permissions sensibly on S3 if you have data sensitivities, as fixing this later can be a difficult task if you have a lot of assets and internal users.
    * 🔹Do create new buckets if you have different data sensitivities, as this is much less error prone than complex permissions rules.
    * 🔹If data is for administrators only, like log data, put it in a bucket that only administrators can access.
    * 💸Limit individual user (or IAM role) access to S3 to the minimal required and catalog the “approved” locations. Otherwise, S3 tends to become the dumping ground where people put data to random locations that are not cleaned up for years, costing you big bucks.
* Manage data lifecycles sensibly.
    * When putting data into a bucket, think about its lifecycle — its end of life, not just its beginning. Rule: data with different expiration policies should be stored under separate prefixes at the top level.
    * For example, some voluminous logs might need to be deleted automatically monthly, while other data is critical and should never be deleted. Having the former in a separate bucket or at least a separate folder is wise.
    * Thinking about this up front will save you pain. It’s very hard to clean up large collections of files created by many engineers with varying lifecycles and no coherent organization.
    * Alternatively you can set a lifecycle policy to archive old data to Glacier. [Be careful](https://alestic.com/2012/12/s3-glacier-costs/) with archiving large numbers of small objects to Glacier, since it may actually cost more.
    * There is also a product called S3 Infrequent Access that has the same durability as Standard S3, but is discounted per GB. It is suitable for objects that are infrequently accessed.
* Creation of objects in S3 is atomic. You’ll never upload a file and have another client see only half the file. Also, if you create a new file, you’ll see it instantly. If you overwrite or delete a file, however, you’re only guaranteed [eventual consistency](https://aws.amazon.com/s3/faqs/#What_data_consistency_model_does_Amazon_S3_employ).
* If you are primarily using a VPC, consider setting up a [VPC Endpoint](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-endpoints.html) for S3 in order to allow your VPC-hosted resources to easily access it without the need for extra network configuration or hops.

### Gotchas and Limitations

* ❗The number of buckets per account is [severely limited](http://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html) (100 per account). Use buckets sparingly. You can ask for a raise in the number of buckets but it will still be capped.
* 🔸Amazon S3 has an [SLA](https://aws.amazon.com/s3/sla/) with 99.9% uptime. If you use S3 heavily, you’ll inevitably see occasional error accessing or storing data as disks or other infrastructure fail. Availability is usually restored in seconds or minutes. Although availability is not extremely high, as mentioned above, durability is excellent.
* 🔸After uploading, any change that you make to the object causes a full rewrite of the object, so avoid appending-like behavior with regular files.
* 🔸Sometimes, S3 suffers from replication issues, when an object is visible from a subset of the machines, depending on which S3 endpoint they hit. Those usually resolve within seconds, however, we’ve seen isolated cases when the issue lingered for 20-30 hours.
* 🔸MD5s and multi-part uploads**: In S3, the [ETag header in S3](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTCommonResponseHeaders.html) is a hash on the object. And in many cases, it is the MD5 hash. However, this [is not the case in general](http://stackoverflow.com/questions/12186993/what-is-the-algorithm-to-compute-the-amazon-s3-etag-for-a-file-larger-than-5gb) when you use multi-part uploads. One workaround is to compute MD5s yourself and put them in a custom header (such as is done by [s4cmd](https://github.com/bloomreach/s4cmd)).
* 🔸**US Standard region:** Most S3 endpoints match the region they’re in, with the exception of the us-east-1 region, which is called 'us-standard' in S3 terminology. This region is also the only region that is replicated across coasts. As a result, latency varies more in this region than in others. You can minimize latency from us-east-1 by using *[s3-external-1.amazonaws.com](http://s3-external-1.amazonaws.com/)*.   


## EC2

### Basics

* **EC2** (Elastic Compute Cloud) is the AWS’ offering of the most fundamental piece of cloud computing: A [virtual private server](https://en.wikipedia.org/wiki/Virtual_private_server). These “instances” and can run [most Linux, BSD, and Windows operating systems](https://aws.amazon.com/ec2/faqs/#What_operating_system_environments_are_supported). Internally, they use [Xen](https://en.wikipedia.org/wiki/Xen) virtualization.
* The term “EC2” is sometimes used to refer to the servers themselves, but technically refers more broadly to a whole collection of supporting services, too, like load balancing (ELBs), IP addresses (EIPs), bootable images (AMIs), security groups, and network drives (EBS) (which we discuss individually in this guide).

### Alternatives and Lock-In

* Running EC2 is akin to running a set of physical servers, as long as you don’t do automatic scaling or tooled cluster setup. If you just run a set of static instances, migrating to another VPS or dedicated server provider should not be too hard.
* 🚪The direct alternatives are Google Cloud, Microsoft Azure, Rackspace, DigitalOcean and other VPS providers, some of which offer similar API for setting up and removing instances.
* **Should you use Amazon Linux?** AWS encourages use of their own [Amazon Linux](https://aws.amazon.com/amazon-linux-ami/), which is evolved from from [Red Hat Enterprise Linux (RHEL)](https://en.wikipedia.org/wiki/Red_Hat_Enterprise_Linux) and [CentOS](https://en.wikipedia.org/wiki/CentOS). It’s used by many, but [others are skeptical](https://www.exratione.com/2014/08/do-not-use-amazon-linux/). Whatever you do, think this decision through carefully. It’s true Amazon Linux is heavily tested and better supported in the unlikely event you have deeper issues with OS and virtualization on EC2. But in general, many companies do just fine using a standard, non-Amazon Linux distribution, such as Ubuntu or CentOS. Using a standard Linux distribution means you have an exactly replicable environment should you use another hosting provider instead of (or in addition to) AWS. It’s also helpful if you wish to test deployments on local developer machines running the same standard Linux distribution (a practice that’s getting more common with Docker, too).

### Tips

* 🔹**Picking regions**: When you first set up, consider which [regions](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-available-regions) you want to use first. Many people in North America just automatically set up in the us-east-1 (N. Virginia) region, which is the default, but it’s worth considering if this is best up front. For example, you might find it preferable to start in us-west-1 (N. California) or us-west-2 (Oregon) if you’re in California and latency matters. Some services [are not available in all regions](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/). Baseline costs also [vary by region](https://aws.amazon.com/ec2/pricing/), up to 10-30%.
* **Instance types**: EC2 instances come in many types, corresponding to the capabilities of the virtual machine in CPU architecture and speed, RAM, disk sizes and types (SSD or magnetic), and network bandwidth.
    * Selecting instance types is complex since there are so many types. Additionally, there are different generations, released [over the years](https://aws.amazon.com/blogs/aws/ec2-instance-history/).
    * 🔹Use the list at [**ec2instances.info**](http://www.ec2instances.info/) to review costs and features. [Amazon’s own list](https://aws.amazon.com/ec2/instance-types/) of instance types is hard to use, and doesn’t list features and price together, which makes it doubly difficult.
    * Prices vary a lot, so use [**ec2instances.info**](http://www.ec2instances.info/) to determine the set of machines that meet your needs and [**ec2price.com**](http://ec2price.com/) to find the cheapest type in the region you’re working in.  Depending on the timing and region, it might be much cheaper to rent an instance with *more* memory or CPU than the bare minimum.
* [**Dedicated instances**](https://aws.amazon.com/ec2/purchasing-options/dedicated-instances/) and [**dedicated hosts**](https://aws.amazon.com/ec2/dedicated-hosts/) are assigned hardware, instead of usual virtual instances. They more expensive than virtual instances but [can be preferable](https://aws.amazon.com/ec2/dedicated-hosts/) for performance, compliance, or licensing reasons.
* **32 bit vs 64 bit**: A few micro, small, and medium instances are still available to use as 32-bit architecture. You’ll be using 64-bit EC2 (“amd64”) instances nowadays, though smaller instances still support 32 bit (“i386”). Use 64 bit unless you have legacy constraints or other good reasons to use 32.
* **HVM vs PV**: There are two kinds of virtualization technology used by EC2, [hardware virtual machine (HVM) and paravirtual (PV)](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/virtualization_types.html). Historically, PV was the usual type, but [now HVM is becoming the standard](https://www.opswat.com/blog/aws-2015-why-you-need-switch-pv-hvm). If you want to use the newest instance types, you must use HVM. See the [instance type matrix](https://aws.amazon.com/amazon-linux-ami/instance-type-matrix/) for details.
* **Operating system**: To use EC2, you’ll need to pick a base operating system. It can be Windows or Linux, such as Ubuntu or [Amazon Linux](https://aws.amazon.com/amazon-linux-ami/). You do this with AMIs, which are covered in more detail in their own section below.
* **Limits**: You can’t create arbitrary numbers of instances. Default limits on numbers of EC2 instances per account vary by instance type, as described in [this list](http://aws.amazon.com/ec2/faqs/#How_many_instances_can_I_run_in_Amazon_EC2).
* Termination protection: For any instances that are important, it is wise to [enable termination protection](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html#Using_ChangingDisableAPITermination).
* **SSH key management**:
    * When you start an instance, you need to have at least one [ssh key pair](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) set up, to bootstrap, i.e., allow you to ssh in the first time.
    * Aside from bootstrapping, you should manage keys yourself on the instances, assigning individual keys to individual users or services as appropriate.
    * Avoid reusing the original boot keys except by administrators when creating new instances.
    * How to avoid sharing keys; how to add individual ssh keys for individual users.
* **GPU support**: You can rent   GPU-enabled instances on EC2. There are [two instance types](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using_cluster_computing.html). Both sport an NVIDIA card (K520,  1536 CUDA cores and M2050, 448 CUDA cores).

### 💸 Cost Management

* With EC2, there is a trade-off between engineering effort (more analysis, more tools, more complex architectures) and spend rate on AWS. If your EC2 costs are small, many of the efforts here are not worth the engineering time required to make them work. But once you know your costs will be growing in excess of an engineer’s salary, serious investment is often worthwhile.
* **Spot instances**: EC2 [spot instances](https://aws.amazon.com/ec2/spot/) are a way to get EC2 resources at significant discount — often many times cheaper than standard on-demand prices — if you’re willing to accept the possibility that they be terminated little to no warning.
* Use spot instances for potentially very significant discounts whenever you can use resources that may be restarted and don’t maintain long-term state.
* The huge savings that you can get with Spot come at the cost of a significant increase in complexity when provisioning and reasoning about the availability of compute capacity.
* Amazon maintains spot prices at a market-driven fluctuating level, based on their inventory of unused capacity. Prices are typically low but can [spike](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-limits.html#spot-bid-limit) very high. See the [price history](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-instances-history.html) to get a sense for this.
* You set a bid price high to indicate how high you’re willing to pay, but you only pay the going rate, not the bid rate. If the market rate exceeds the bid, your instance may be terminated.
* Prices are per instance type and per availability zone. The same instance type may have wildly different price in different zones at the same time. Different instance types can have very different prices, even for similarly powered instance types in the same zone.
* Compare prices across instance types for better deals.
* Use spot instances whenever possible. Setting a high bid price will assure your machines stay up the vast majority of the time, at a fraction of the price of normal instances.
* Get notified up to two minutes before price-triggered shutdown by polling [your spot instances’ metadata](https://aws.amazon.com/blogs/aws/new-ec2-spot-instance-termination-notices/).
* **Spot fleet**: You can realize even bigger cost reductions at the same time as improvements to fleet stability relative to regular spot usage by using [Spot fleet](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-fleet.html) to bid on instances across instance types, availability zones, and (through multiple Spot Fleet Requests) regions.
    * Spot fleet targets maintaining a  specified (and weighted-by-instance-type) total capacity across a cluster of servers. If the spot price of one instance type and availability zone combination rises above the weighted bid, it will rotate running instances out and bring up new ones of another type and location up in order to maintain the target capacity without going over target cluster cost.
* Make sure your usage profile works well for Spot before investing heavily in tools to manage a particular configuration.
* It is often wise to employ **third-party services to manage costs **— see above.
* **Reserved Instances** allow you to get significant discounts on EC2 compute hours in return for a commitment to pay for instance hours of a specific instance type in a specific AWS region and availability zone for a pre-established time frame (1 or 3 years). Further discounts can be realized through “partial” or “all upfront” payment options.
* Consider using Reserved Instances when you can predict your longer-term compute needs and need a stronger guarantee of compute availability and continuity than the (typically cheaper) spot market can provide. However be aware that if your architecture changes your computing needs may change as well so long term contracts can seem attractive but may turn out to be cumbersome.
* Instance reservations are not tied to specific EC2 instances - they are applied at the billing level to eligible compute hours as they are consumed across all of the instances in an account.
* If you have multiple AWS accounts and have configured them to roll charges up to one account using the “Consolidated Billing” feature, you can expect _unused_ Reserved Instance hours from one account to be applied to matching (region, availability zone, instance type) compute hours from another account.
* If you have multiple AWS accounts that are linked with Consolidated Billing, plan on using reservations, and want unused reservation capacity to be able to apply to compute hours from other accounts, you’ll need to create your instances in the availability zone with the same _name_ across accounts. Keep in mind that when you have done this, your instances may not end up in the same _physical_ data center across accounts - Amazon shuffles availability zones names across accounts in order to equalize resource utilization.

### Gotchas and Limitations

* ❗Never use ssh passwords. Just don’t do it; they are too insecure, and consequences of compromise too severe. Use keys instead. [Read up on this](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2) and fully disable ssh password access to your ssh server by making sure 'PasswordAuthentication no' is in your /etc/ssh/sshd_config file.  If you’re careful about managing ssh private keys everywhere they are stored, it is a major improvement on security over password-based authentication.
* 🔸For all [newer instance types](https://aws.amazon.com/amazon-linux-ami/instance-type-matrix/), when selecting the AMI to use, be sure you select the HVM AMI, or it just won’t work.
* ❗When creating an instance and using a new ssh key pair, [make sure the ssh key permissions are correct](http://stackoverflow.com/questions/1454629/aws-ssh-access-permission-denied-publickey-issue).
* 🔸Sometimes certain EC2 instances can get scheduled for retirement by AWS due to “detected degradation of the underlying hardware,” in which case you are given a couple of weeks to migrate to a new instance.
* 🔸Periodically you may find that your server or load balancer is receiving traffic for (presumably) a previous EC2 server that was running at the same IP address that you are handed out now (this may not matter, or it can be fixed by migrating to another new instance).
* ❗If the EC2 API  itself is a critical dependency of your infrastructure (e.g. for automated server replacement,  custom scaling algorithms, etc.) and you are running at a large scale or making many EC2 API calls, make sure that you understand when they might fail (calls to it are [rate limited](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/query-api-troubleshooting.html#api-request-rate) and the limits are not published and subject to change) and code and test against that possibility.
* ❗Many newer EC2 instance types are EBS-only. Make sure to factor in EBS performance and costs when planning to use them.


## AMIs

### Tips

* [**Amazon Machine Images (AMIs)**](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) are immutable images that are used to launch preconfigured EC2 instances. They come in both public and private flavors. Access to public AMIs is either freely available (shared/community AMIs) or bought and sold in the [**AWS Marketplace**](http://aws.amazon.com/marketplace).
* Many operating system vendors publish ready-to-use base AMIs. For Ubuntu, see the [Ubuntu AMI Finder](https://cloud-images.ubuntu.com/locator/ec2/). Amazon of course has [AMIs for Amazon Linux](https://aws.amazon.com/amazon-linux-ami/).
* AMIs are built independently based on how they will be deployed. You must select AMIs that match your deployment when using them or creating them:
    * EBS or instance store
    * PV or HVM [virtualization types](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/virtualization_types.html)
    * 32 bit (“i386”) vs 64 bit (“amd64”) architecture
* As discussed above, modern deployments will usually be with *64-bit EBS-backed HVM.*
* You can create your own custom AMI by [snapshotting the state](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/creating-an-ami-ebs.html) of an EC2 instance that you have modified.
* [AMIs backed by EBS storage](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ComponentsAMIs.html#storage-for-the-root-device) have the necessary image data loaded into the EBS volume itself and don’t require an extra pull from S3, which results in EBS-backed instances coming up much faster than instance storage-backed ones.
* *AMIs are per region*, so you must look up AMIs in your region, or copy your AMIs between regions with the [AMI Copy](https://aws.amazon.com/about-aws/whats-new/2013/03/12/announcing-ami-copy-for-amazon-ec2/) feature.
* As with other AWS resources, it’s wise to [use tags](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html) to version AMIs and manage their lifecycle.
* If you create your own AMIs, there is always some tension in choosing how much installation and configuration you want to “bake” into them.
    * Baking less into your AMIs (for example, just a configuration management client that downloads, installs, and configures software on new EC2 instances when they are launched) allows you to minimize time spent automating AMI creation and managing the AMI lifecycle (you will likely be able to use fewer AMIs and will probably not need to update them as frequently), but results in longer waits before new instances are ready for use and results in a higher chance of launch-time installation or configuration failures.
    * Baking more into your AMIs (for example, pre-installing but not fully configuring common software along with a configuration management client that loads configuration settings at launch time) results in a faster launch time and fewer opportunities for your software installation and configuration to break at instance launch time but increases the need for you to create and manage a robust AMI creation pipeline.
    * Baking even more into your AMIs (for example, installing all required software as well and potentially also environment-specific configuration information) results in fast launch times and a much lower chance of instance launch-time failures but (without additional re-deployment and re-configuration considerations) can require time consuming AMI updates in order to update software or configuration as well as more complex AMI creation automation processes.
* Which option you favor depends on how quickly you need to scale up capacity, and size and maturity of your team and product.
    * When instances boot fast, auto-scaled services require less spare capacity built in and can more quickly scale up in response to sudden increases in load. When setting up a service with autoscaling, consider baking more into your AMIs and backing them with the EBS storage option.
    * As systems become larger, it common to have more complex AMI management, such as a multi-stage AMI creation process in which few (ideally one) common base AMIs are infrequently regenerated when components that are common to all deployed services are updated and then a more frequently run “service-level” AMI generation process that includes installation and possibly configuration of application-specific software.
* More thinking on AMI creation strategies [here](http://techblog.netflix.com/2013/03/ami-creation-with-aminator.html).
* Use tools like [Packer](https://packer.io/) to simplify and automate AMI creation.


## EBS

### Tips

* ⏱**RAID**: Use [RAID drives](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/raid-config.html) for [increased performance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSPerformance.html).
* ⏱A worthy read is AWS’ [post on EBS IO characteristics](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-io-characteristics.html) as well as their [performance tips](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSPerformance.html#d0e86148).
* ⏱One can [provision IOPS](http://aws.amazon.com/ebs/details/) (that is, pay for a specific level of I/O operations per second) to ensure a particular level of performance for a disk.
* ⏱A single EBS volume allows 10k IOPS max. To get the maximum performance out of an EBS volume, it has to be of a maximum size and attached to an EBS-optimized EC2 instance.
* A standard block size for an EBS volume is 16kb.

### Gotchas and Limitations

* ❗EBS durability is reasonably good for a regular hardware drive (annual failure rate of [between 0.1% - 0.2%](http://aws.amazon.com/ebs/details/#availabilityanddurability)). On the other hand, that is very poor if you don’t have backups! By contrast, S3 durability is extremely high. *If you care about your data, back it up S3 with snapshots.*
* 🔸EBS has an [**SLA**](http://aws.amazon.com/ec2/sla/) with **99.95%** uptime. See notes on high availability below.
* ❗EBS volumes have a [**volume type**](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html) indicating the physical storage type. The types called “standard” (**st1** or **sc1**) actually old spinning-platter disks, which deliver only hundreds of IOPS — not what you want unless you’re really trying to cut costs. Modern SSD-based **gp2** or **io1** are typically the options you want.


## ELBs

### Tips

* The [elastic load balancer](https://aws.amazon.com/elasticloadbalancing/) (ELB) is AWS’ load balancing product. They’re great for common load balancing situations. They support TCP, HTTP, and SSL termination.
* If you don’t have opinions on your load balancing up front, and don’t have complex load balancing needs like application-specific routing of requests, it’s reasonable just to use an ELB for load balancing instead.
* Even if you don’t want to think about load balancing at all, because your architecture is so simple (say, just one server), put an ELB in front of it anyway. This gives you more flexibility when upgrading, since you won’t have to change any DNS settings that will be slow to propagate, and also it lets you do a few things like terminate SSL more easily.
* **ELBs have many IPs**: Internally, an ELB is simply a collection of individual software load balancers hosted within EC2, with DNS load balancing traffic among them. The pool can contain many IPs, at least one per availability zone, and depending on traffic levels. They also support SSL termination, which is very convenient.
* For single-instance deployments, you might consider just assigning an elastic IP to an instance, but it’s generally quicker to add or remove instances from an ELB than to reassign an elastic IP.
* **Best practices**: [This article](http://aws.amazon.com/articles/1636185810492479) is a must-read if you use ELBs heavily, and has a lot more detail.
* **Scaling**: ELBs can scale to very high throughput, but scaling up is not instantaneous. If you’re planning to be hit with a lot of traffic suddenly, it can make sense to load test them so they scale up in advance. You can also [contact Amazon](http://aws.amazon.com/articles/1636185810492479) and have them “pre-warm” the load balancer.
* **Client IPs**: In general, if servers want to know true client IP addresses, load balancers must forward this information somehow. ELBs add the standard [X-Forwarded-For](https://en.wikipedia.org/wiki/X-Forwarded-For) header. When using an ELB as an HTTP load balancer, it’s possible to get the client’s IP address from this.
* **Websockets** and **HTTP2/SPDY** are not currently supported directly. But you can use TCP instead of HTTP as the protocol to make it work. More details [here](http://www.quora.com/When-will-Amazon-ELB-offer-SPDY-support). You’ll want to [enable the obscure but useful Proxy Protocol](http://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/enable-proxy-protocol.html) ([more on this](https://chrislea.com/2014/03/20/using-proxy-protocol-nginx/)) to pass client IPs over a TCP load balancer.
* Flip load balancer after spinning up a new stack with your latest version, keep old stack running for one or two hours, and either flip back to old stack in case of problems or tear down it down.

### Gotchas and Limitations

* In general, ELBs are not as “smart” as some load balancers, and don’t have fancy features or fine-grained control a traditional hardware load balancer would offer. For most common cases involving sessionless apps or cookie-based sessions over HTTP, or SSL termination, they work well.
* Complex rules for directing traffic are not supported. For example, you can’t direct traffic based on a regular expression in the URL, like [HAProxy](http://www.haproxy.org/) offers.
* **Apex DNS names**: Once upon a time, you couldn’t assign an ELB to an apex DNS record (i.e. example.com instead of foo.example.com) because it needed to be an A record instead of a CNAME. This is now possible with a Route 53 alias record directly pointing to the load balancer.
* ❗ELBs have **no fixed external IP** that all clients see. For most consumer apps this doesn’t matter, but enterprise customers of yours may want this. IPs will be different for each user, and will vary unpredictably for a single client over time (within the standard [EC2 IP ranges](http://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html)).
* ❗Some web clients or reverse proxies cache DNS lookups for a long time, which is problematic for ELBs, since they change their IPs. This means after a few minutes, hours, or days, your client will stop working, unless you disable DNS caching. Watch out for [Java’s settings](http://docs.oracle.com/javase/8/docs/api/java/net/InetAddress.html) and be sure to [adjust them properly](http://docs.aws.amazon.com/AWSSdkDocsJava/latest/DeveloperGuide/java-dg-jvm-ttl.html). Another example is nginx as a reverse proxy, which [resolves backends only at start-up](https://www.jethrocarr.com/2013/11/02/nginx-reverse-proxies-and-dns-resolution/).
* ❗It’s not unheard of for IPs to be recycled between customers without a long cool-off period. So as a client, if you cache an IP and are not using SSL (to verify the server), you might get not just errors, but responses from completely different services or companies!
* 🔸As an operator of a service behind an ELB, the latter phenomenon means you can also see puzzling or erroneous requests by clients of other companies. This is most common with clients using back-end APIs (since web browsers typically cache for a limited period).
* 🔸ELBs use [HTTP keep-alives](https://en.wikipedia.org/wiki/HTTP_persistent_connection) on the internal side. This can cause an unexpected side effect: Requests from different clients, each in their own TCP connection on the external side, can end up on the same TCP connection on the internal side. Never assume that multiple requests on the same TCP connection are from the same client!
* ❗ELB takes time to scale up, it does not handle sudden spikes in traffic well. Therefore, if you anticipate a spike, you need to “pre-warm” the ELB by gradually sending an increasing amount of traffic.


## Elastic IPs

### Tips

* Elastic IPs are limited to 5 per account. It’s possible to [request more](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-elastic-ips-ec2-classic).
* If an Elastic IP is not attached to an active resource there is a small [hourly fee](https://aws.amazon.com/ec2/pricing/#Elastic_IP_Addresses).



## Glacier

### Tips

* You can physically [ship](https://aws.amazon.com/blogs/aws/send-us-that-data/) your data to Amazon to put on Glacier on a USB or eSATA HDD.

### Gotchas and Limitations

* Getting files off Glacier is glacially slow (on the order of 5-6 hours).
* Due to a fixed overhead per file (you pay per PUT or GET operation), uploading and downloading many small files on/to Glacier might be very expensive. There is also a 32k storage overhead per file. Hence a good idea is to archive files before upload.
* Glacier’s pricing policy is reportedly pretty complicated: “Glacier data retrievals are priced based on the peak hourly retrieval capacity used within a calendar month.”  Some more info can be found [here](https://medium.com/@karppinen/how-i-ended-up-paying-150-for-a-single-60gb-download-from-amazon-glacier-6cb77b288c3e#.wjl4dbgza) and [here](https://news.ycombinator.com/item?id=10921365).


## RDS

### Tips

* If you’re looking for the managed convenience of RDS for MongoDB, this isn’t offered by AWS directly, but you may wish to consider a provider such as [**mLab**](https://mlab.com/).
* MySQL RDS allows access to [binary logs](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.Concepts.MySQL.html#USER_LogAccess.MySQL.BinaryFormat).

### Gotchas and Limitations

* RDS instances run on EBS volumes, and hence are constrained by the EBS performance.
* ⏱RDS instances run on EBS volumes, and hence are constrained by the EBS performance.
* 🔸Verify what database features you need, as not everything you might want is available on RDS. For example, if you are using Postgres, check the list of [supported features and extensions](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_PostgreSQL.html#SQLServer.Concepts.General.FeatureSupport). If the features you need aren’t supported by RDS, you’ll have to deploy your database yourself.
* 🔸If you prefer a MySQL-style database but are starting something new, don’t use MySQL on RDS. Use **Aurora** instead of RDS for increased availability. It’s the next-generation solution.


## DynamoDB

### Basics

* DynamoDB is a NoSQL database with focuses on speed, flexibility and scalability.
* DynamoDB is priced on a combination of throughput and storage.

### Alternatives and Lock-in

* ⛓ Unlike the technologies behind many other Amazon products, DynamoDB is a proprietary AWS product with no interface-compatible alternative available as an open source project. If you tightly couple your application to its API and featureset, it will take significant effort to replace.
* The most commonly used alternative to DynamoDB is [Cassandra](http://cassandra.apache.org/).

### Tips

* There is a [local version](https://aws.amazon.com/blogs/aws/dynamodb-local-for-desktop-development/) of DynamoDB provided for developer use.
* [DynamoDB Streams](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html) provides an ordered stream of changes to a table. Use it to replicate, back up, or drive events off of data
* DynamoDB can be used [as a simple locking service](https://gist.github.com/ryandotsmith/c95fd21fab91b0823328).

### Gotchas and Limitations

* 🔸 DynamoDB doesn’t provide a way to bulk-load data, and this has some [unfortunate consequences](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GuidelinesForTables.html#GuidelinesForTables.AvoidExcessivePTIncreases). Since you need to use the regular service APIs to update existing or create new rows, it is common to temporarily turn up a destination table’s write throughput to speed import. But when the table’s write capacity is increased, DynamoDB may do an irreversible split of the partitions underlying the table, spreading the total table capacity evenly across the new generation of tables. Later, if the capacity is reduced, the capacity for each partition is also reduced but the total number of partitions is not, leaving less capacity for each partition. This leaves the table in a state where it much easier for hotspots to overwhelm individual partitions.
* It is important to make sure that DynamoDB [resource limits](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Limits.html#limits-data-types) are compatible with your dataset and workload. For example, the maximum size value that can be added to a DynamoDB table is 400 KB.


## ECS

### Basics

* [ECS](https://aws.amazon.com/ecs/) (EC2 Container Service) is a relatively new service (launched end of 2014) that manages clusters of services deployed via Docker.
* See the [Containers and AWS](#containers-and-aws) section for more context on containers.
* ECS is growing in adoption, especially for companies that embrace microservices.
* Deploying Docker directly in EC2 yourself is another common approach to using Docker on AWS. Using ECS is not required, and ECS does not (yet) seem to be the predominant way many companies are using Docker on AWS.
* It’s also possible to use [Elastic Beanstalk with Docker](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_docker.html), which is reasonable if you’re already using Elastic Beanstalk.
* Using Docker may change the way your services are deployed within EC2 or Elastic Beanstalk, but it does not radically change how most other services are used.
* [ECR](https://aws.amazon.com/ecr/) (EC2 Container Registry) is Amazon’s managed Docker registry service. While simpler than running your own registry, it is missing some features that might be desired by some users:
    * Doesn’t support cross-region replication of images.
        * If you want fast fleet-wide pulls of large images, you’ll need to push your image into a region-local registry.
    * Doesn’t support custom domains / certificates.

[🚧 Please help expand this section.]


## Lambda

### Basics

* Lambda is a relatively new service (launched at end of 2014) that offers a different type of compute abstraction: A user-defined function that can perform a small operation, where AWS manages provisioning and scheduling how it is run.
* This abstraction has grown to be called “serverless” since you don't explicitly manage server inistances, as with EC2. (This term is a bit confusing since the functions themselves do of course run on servers managed by AWS.)
* Adoption of Lambda has grown very rapidly in 2015, with many use cases that traditionally would be solved by managing EC2 services migrating to serverless architectures.
* The [Awesome Serverless](https://github.com/anaibol/awesome-serverless) list gives a good set of examples of modern tools.

[🚧 Please help expand this section.]


## Route 53

### Alternatives and Lock-In

* Historically, AWS was slow to penetrate the DNS market (as it is often driven by perceived reliability and long-term vendor relationships) but Route 53 has matured and [is becoming the standard option](https://www.datanyze.com/market-share/dns/) for many companies. Route 53 is cheap by historic DNS standards, as it has a fairly large global network with geographic DNS and other formerly “premium” features. It’s convenient if you are already using AWS.
* ⛓Generally you don’t get locked into a DNS provider for simple use cases, but increasingly become tied in once you use specific features like geographic routing or Route 53’s alias records.
* 🚪Many alternative DNS providers exist, ranging from long-standing premium brands like [UltraDNS](https://www.neustar.biz/services/dns-services) and [Dyn](http://dyn.com/managed-dns/) to less well known, more modestly priced brands like [DNSMadeEasy](http://www.dnsmadeeasy.com/). Most DNS experts will tell you that the market is opaque enough that reliability and performance don’t really correlate well with price.
* ⏱Route 53 is usually somewhere in the middle of the pack on performance tests, e.g. the [SolveDNS reports](http://www.solvedns.com/dns-comparison/).

### Tips

* Know about Route 53’s “alias” records:
    * Route 53 supports all the standard DNS record types, but note that [**alias resource record sets**](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-choosing-alias-non-alias.html) are not standard part of DNS, but a specific Route 53 feature. (It’s available from other DNS providers too, but each provider has a different name for it.)
    * Aliases are like an internal name (a bit like a CNAME) that is resolved internally on the server side. For example, traditionally you could have a CNAME to the DNS name of an ELB, but it’s often better to make an alias to the same ELB. The effect is the same, but in the latter case, externally, all a client sees is the target the record points to.
    * It’s often wise to use alias record as an alternative to CNAMEs, since they can be updated instantly with an API call, without worrying about DNS propagation.
    * You can use them for ELBs or any other resource where AWS supports it.
    * Somewhat confusingly, you can have CNAME and A aliases, depending on the type of the target.
    * Because aliases are extensions to regular DNS records, if exported, the output [zone file](https://en.wikipedia.org/wiki/Zone_file) will have additional non-standard “ALIAS” lines in it.
* Take advantage of AWS Route 53 latency based routing. This means that your users around the globe are automatically directed to the nearest AWS region where you are running in terms of having the shortest latency.


## CloudFormation

### Basics

* CloudFormation promises a way to save, templatize, and reproduce entire configurations.

### Alternatives and Lock-In

* Hashicorp’s [Terraform](https://www.terraform.io/intro/vs/cloudformation.html) is a third-party alternative.

### Tips

* [Troposphere](https://github.com/cloudtools/troposphere) is a Python library that makes it much easier to create CloudFormation templates.

### Gotchas and Limitations

* 🔸Many users don’t use CloudFormation at all because of its limitations, or because they find other solutions preferable:
    * CloudFormation syntax is a confusing JSON format that makes both reading and debugging difficult.
    * To use it effectively often involves additional tooling, such as converting it to YAML or using Troposphere.
    * It’s hard to assemble good CloudFormation configurations from existing state. AWS does [offer a trick to do this](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-using-cloudformer.html), but it’s very clumsy.
    * Often there are other ways to accomplish the same goals, such as local scripts (Boto, Bash, Ansible, etc.) you manage yourself that build infrastructure, or Docker-based solutions ([Convox](https://convox.com/), etc.).
    * It is *very* slow for anything that isn’t a trivial example, as it essentially does not parallelize any of the resource creation.
    * Many companies do use CloudFormation, but usually with extensive investment.


## VPCs, Network Security, and Security Groups

### Tips

* **Security groups** are your first line of defense for your servers. Be extremely restrictive of what ports are open to all incoming connections. In general, if you use ELBs or other load balancing, the only ports that need to be open to incoming traffic would be port 22 and whatever port your application uses.
* **Port hygiene**: A good habit is to pick unique ports within an unusual range for each different kind of production service. For example, your web fronted might use 3010, your backend services 3020 and 3021, and your Postgres instances the usual 5432. Then make sure you have fine-grained security groups for each set of servers. This makes you disciplined about listing out your services, but also is more error-proof. For example, should you accidentally have an extra Apache server running on the default port 80 on a backend server, it will not be exposed.
* All modern AWS accounts (those created [after 2013-12-04](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-vpc.html)) are “EC2-VPC” accounts that support VPCs, and all instances will be in a default VPC. Older accounts may still be using “EC2-Classic” mode. Some features don’t work without VPCs, so you probably will want to [migrate](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/vpc-migrate.html).
* For migrating from older EC2-Classic deployments to modern EC2-VPC setup, [this article](http://blog.kiip.me/engineering/ec2-to-vpc-executing-a-zero-downtime-migration/) may be of help.
* For basic AWS use, one default VPC may be sufficient. But as you scale up, you should consider mapping out network topology more thoroughly. A good overview of best practices is [here](http://blog.flux7.com/blogs/aws/vpc-best-configuration-practices).
* Consider controlling access to your private AWS resources through a [VPN](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpn-connections.html).
    * You get better visibility into and control of connection and connection attempts.
    * You expose a smaller surface area for attack compared to exposing separate (potentially authenticated) services over the public internet.
        * e.g. A bug in the YAML parser used by the Ruby on Rails admin site is much less serious when the admin site is only visible to the private network and accessed through VPN.
    * Another common pattern (especially as deployments get larger, security or regulatory requirements get more stringent, or team sizes increase) is to provide a [bastion host](https://www.pandastrike.com/posts/20141113-bastion-hosts) behind a VPN through which all SSH connections need to transit.

### Gotchas and Limitations

* 🔸Security groups are not shared across data centers, so if you have infrastructure in multiple data centers, you should make sure your configuration/deployment tools take that into account.
* ❗Be careful when choosing your VPC IP CIDR block: If you are going to need to make use of [ClassicLink](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/vpc-classiclink.html), make sure that your private IP range [doesn’t overlap](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/vpc-classiclink.html#classiclink-limitations) with that of EC2 Classic.
* ❗If you are going to peer VPCs, carefully consider the cost of of [data transfer between VPCs](https://aws.amazon.com/vpc/faqs/#Peering_Connections), since for some workloads and integrations, this can be prohibitively expensive.


## CloudFront

### Basics

* [CloudFront](https://aws.amazon.com/cloudfront/) is AWS’ [content delivery network (CDN)](https://en.wikipedia.org/wiki/Content_delivery_network).
* Its primary use is improving latency for end users in to accessing cacheable content by hosting it at [about 40 global edge locations](http://aws.amazon.com/cloudfront/details/).

### Alternatives and Lock-in

* 🚪CDNs are [a highly fragmented market](https://www.datanyze.com/market-share/cdn/). CloudFront has grown to be a leader, but many alternatives that might better suit specific needs.

### Tips

* In its basic version, CloudFront [supports SSL](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/SecureConnections.html) via the [SNI extension to TLS](https://en.wikipedia.org/wiki/Server_Name_Indication), which is supported by all modern web browsers. If you need to support older browsers, you need to pay a few hundred dollars a month for dedicated IPs.
    * 💸⏱Consider invalidation needs carefully. CloudFront [does support invalidation](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Invalidation.html) of objects from edge locations, but this typically takes many minutes to propagate to edge locations, and costs $0.005 per request after the first 1000 requests. (Some other CDNs support this better.)
* Everyone should use TLS nowadays if possible. [Ilya Grigorik’s table](https://istlsfastyet.com/#cdn-paas) offers a good summary of features regarding TLS performance features of CloudFront.
* An alternative to invalidation that is often easier to manage, and instant, is to configure the distribution to [cache with query strings](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/QueryStringParameters.html) and then append unique query strings with versions onto assets that are updated frequently.
* ⏱For good web performance, it’s important turn on the option to [enable compression](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/ServingCompressedFiles.html) on CloudFront distributions if the origin is S3 or another source that does not already compress.

### Gotchas and Limitations

* HTTP/2 is not yet supported.
* If using S3 as a backing store, remember that the endpoints for website hosting and for general S3 are different. Example: “bucketname.s3.amazonaws.com” is a standard S3 serving endpoint, but to have redirect and error page support, you need to use the website hosting endpoint listed for that bucket, e.g. “bucketname.s3-website-us-east-1.amazonaws.com” (or the appropriate region).

## DirectConnect

### Tips

* Direct Connect is a private, dedicated connection from your network(s) to AWS.
    * If your data center has [a partnering relationship](https://aws.amazon.com/directconnect/partners/) with AWS, this process is streamlined.
* Use for more consistent predictable network performance guarantees.
    * 1 Gbps or 10 Gbps per link
* Use to peer your colocation, corporate, or physical datacenter network with your VPC(s).
    * Example: Extend corporate LDAP and/or Kerberos to EC2 instances running in a VPC.
    * Example: Make services that are hosted outside of AWS for financial, regulatory, or legacy reasons callable from within a VPC.


## High Availability

### Tips

* AWS offers two levels of redundancy, [regions and availability zones (AZs)](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-regions-availability-zones).
* When used correctly, regions and zones do allow for high availability. You may want to use non-AWS providers for larger business risk mitigation (i.e. not tying your company to one vendor), but reliability of AWS across regions is very high.
* **Multiple regions**: Using multiple regions is complex, since it’s essentially like completely separate infrastructure. It is necessary for business-critical services which highest levels of redundancy. However, for many applications (like your average consumer startup), deploying extensive redundancy across regions may be overkill.
* The [High Scalability Blog](http://highscalability.com/blog/2016/1/11/a-beginners-guide-to-scaling-to-11-million-users-on-amazons.html) has a good guide to help you understand when you need to scale an application to multiple regions.
* 🔹**Multiple AZs**: Using AZs wisely is the primary tool for high availability!
    * The bulk of outages in AWS services affect one zone only. There have been rare outages affecting multiple zones simultaneously (for example, the [great EBS failure of 2011](http://aws.amazon.com/message/65648/)) but in general most customers’ outages are due to using only a single AZ for some infrastructure.
    * Consequently, design your architecture to minimize the impact of AZ outages, especially single-zone outages.
    * Deploy key infrastructure across at least two or three AZs. Replicating a single resource across more than three zones often won’t make sense if you have other backup mechanisms in place, like S3 snapshots.
    * Deploy instances evenly across all available AZs, so that only a minimal fraction of your capacity is lost in case of an AZ outage.
    * If your architecture has single points of failure, put all of them into a single AZ. This may seem counter-intuitive, but it minimizes the likelihood of any one SPOF to go down on an outage of a single AZ.
* **EBS vs instance storage**: For a number of years, EBSs had a poorer track record for availability than instance storage. For systems where individual instances can be killed and restarted easily, instance storage with sufficient redundancy could give higher availability overall. EBS has improved, and modern instance types (since 2015) are now EBS-only, so this approach, while helpful at one time, may be increasingly archaic.
* Be sure you use and understand **ELBs** whenever appropriate. (See the section on ELBs.) Many outages are due to not using load balancers, or misunderstandings or misconfigurations of ELBs.

### Gotchas and Limitations

* **AZ naming** differs from one customer account to the next. Your “us-west-1a” is not the same as another customer’s “us-west-1a” — the letters are assigned to physical AZs randomly per account. This can also be a gotcha if you have multiple AWS accounts.
* **Cross-AZ traffic** is not free. At large scale, the costs add up to a significant amount of money. If possible, optimize your traffic to stay within the same AZ as much as possible.  


## Redshift

### Tips

* [Redshift](https://aws.amazon.com/redshift/) is AWS’ data warehouse solution (built on top of [ParAccel](http://www.actian.com/)), which is highly parallel, share-nothing and columnar. It is very widely used.
* Redshift is based on Postgres, but its SQL dialect and performance profile are different.
    * Redshift supports only [11 primitive data types](https://docs.aws.amazon.com/redshift/latest/dg/c_Supported_data_types.html). ([List of unsupported Postgres types](https://docs.aws.amazon.com/redshift/latest/dg/c_unsupported-postgresql-datatypes.html))
    * It has a leader node and computation nodes (the leader node distributes queries to the computation ones). Note that some functions [can be executed only on the lead node.](https://docs.aws.amazon.com/redshift/latest/dg/c_SQL_functions_leader_node_only.html)
    * 🔸Redshift does not support many Postgres functions, most notable date/time related or aggregates. See the [full list here](https://docs.aws.amazon.com/redshift/latest/dg/c_unsupported-postgresql-functions.html).
* Major 3rd-party BI tools support Redshift integration (see [Quora](https://www.quora.com/Which-BI-visualisation-solution-goes-best-with-Redshift)).

### Gotchas and Limitations

* 🔸While Redshift can handle heavy queries well, it does not scale horizontally, i.e. does not handle multiple queries in parallel. Therefore, if you expect a high parallel load, consider replicating or (if possible) sharding your data across multiple clusters.
* Redshift data commit transactions are very expensive and serialized at the cluster level. Therefore, consider grouping multiple COPY commands into a single transaction whenever possible.
* 🔸Redshift does not support multi-AZ deployments. Building multi-AZ clusters is not trivial. [Here ](https://blogs.aws.amazon.com/bigdata/post/Tx13ZDHZANSX9UX/Building-Multi-AZ-or-Multi-Region-Amazon-Redshift-Clusters)is an example using Kinesis.
* 🔸Redshift has reserved keywords which are not present in Postgres (see full list [here](https://docs.aws.amazon.com/redshift/latest/dg/r_pg_keywords.html)). Watch out for DELTA ([Delta Encodings](https://docs.aws.amazon.com/redshift/latest/dg/c_Delta_encoding.html)).


## EMR

### Tips

* EMR relies on many versions of Hadoop and other supporting software. Be sure to check [which versions are in use](https://docs.aws.amazon.com/ElasticMapReduce/latest/ReleaseGuide/emr-release-components.html).
* **EMR costs** can pile up quickly.  [This blog post](http://engineering.bloomreach.com/strategies-for-reducing-your-amazon-emr-costs/) has some tips.
* ⏱Off-the-shelf EMR and Hadoop can have significant overhead when compared with efficient processing on a single machine. If your data is small and performance matters, you may wish to consider alternatives, as [this post](http://aadrake.com/command-line-tools-can-be-235x-faster-than-your-hadoop-cluster.html) illustrates.
* Python programmers may want to take a look at Yelp’s [mrjob](https://github.com/Yelp/mrjob).
* It takes time to tune performance of EMR jobs, which is why third-party services such as [Qubole’s data service](https://www.qubole.com/mapreduce-as-a-service/) are gaining popularity as ways to improve performance or reduce costs.


## Further Reading

This section covers a few unusually useful or “must know about” resources or lists.

* AWS
    * [AWS In Plain English](https://www.expeditedssl.com/aws-in-plain-english): A readable overview of all the AWS services.
    * [Awesome AWS](https://github.com/donnemartin/awesome-aws): A curated list of AWS tools and software
* General references
    * [Awesome Microservices](https://github.com/mfornos/awesome-microservices): A curated list of tools and technologies for microservice architectures. Worth browsing to learn about popular open source projects.
    * [Is it fast yet?](https://istlsfastyet.com/): Ilya Grigorik’s TLS performance overview
    * [High Performance Browser Networking](https://hpbn.co/): A full, modern book on web network performance; a presentation on the HTTP/2 portion is [here](https://docs.google.com/presentation/d/1r7QXGYOLCh4fcUq0jDdDwKJWNqWK1o4xMtYpKZCJYjM/edit?usp=sharing).


## Disclaimer

The authors and contributors to this content cannot guarantee the validity of the information found here. Please make sure that you understand that the information provided here is being provided freely, and that no kind of agreement or contract is created between you and any persons associated with this content or project. The authors and contributors do not assume and hereby disclaim any liability to any party for any loss, damage, or disruption caused by errors or omissions in the information contained in, associated with, or linked from this content, whether such errors or omissions result from negligence, accident, or any other cause.


## License

[![Creative Commons License](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-sa/4.0/)

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).
