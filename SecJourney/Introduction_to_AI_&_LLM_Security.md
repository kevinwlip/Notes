# Introduction to AI/LLM Security

1. Explain the differences between Data, Model, and Application engineering security in the AI Lifecycle.
2. Define the terminology and nuances between AI, ML, and LLM.
3. Understand the importance of securing Large Language Models and other machine-learning system software.
4. List the properties of a secure ML system.

## Why do we care?
Large-scale adoption of Artificial Intelligence and LLM tools as part of modern Internet infrastructure.
LLM and AI are rapidly being adopted across industries from medical to industrial. As the trend continues, and the more this adoption takes place, it will keep expanding the threat landscape and possible security vulnerabilities that these applications can fall victim to.

It is essential to understand the multiple ways to secure AI/LLM systems, especially if they are going to be used in the enterprise and as a core business tool. Before expanding into this idea, it is best to define some terms and nuanced differences that are important to understand.

## Artificial Intelligence vs Machine Learning
Two diagrams and definitions for Artificial Intelligence and Machine learning along with specific types of each.
Artificial intelligence, or AI for short, can be thought of as knowledge of a model that is applied to certain tasks. There are two types of Artificial Intelligence. You have specific AI and generic AI. Generic AI can be thought of as like chatGPT; it is broad and has a lot of use cases. Specific AI usually concentrates on just one problem.

There is an overlap between the terms artificial intelligence and machine learning, but to put it simply, machine learning is computer software learning to adapt without explicit instructions from humans. This type of technique can be broken down further into both unsupervised and supervised learning categories.

Unsupervised is more of the chatGPT, where we have unlabeled data. And this is just defining and trying to really gain insight into the relationships between that data. And then supervised machine learning is when you have very clean, structured labeled data from the data prep phase and do that for a specific type.

## Large Language Model (LLMs)
Two diagrams and definitions for Neural networks and large language models, expanding into the characteristics and architecture of each.
Neural networks are a type of machine learning process and can be thought of as your brain. There are a lot of neurons and nodes that are all interconnected with multiple layers. These nodes have inherent relationships with each other and filtered scoring that serve to either strengthen or loosen that relationship.

A large language model is a type of neural network. This type of neural network uses transformer architecture, which can be thought of as a sequence of steps, utilizing one-step commands from one to the next. It also uses an encoder and decoder structure; this kind of architecture is innovative because it gives the model self-attention. This can be thought of as memory and is going to allow the user to steer the output from this tool for specific purposes.

## Large Language Model (LLM) OWASP Top 10
Ten icons are listed in order showing the Large Language Model (LLM) OWASP Top 10 which goes from prompt injection to model theft. 
When looking at security around these types of systems, the first place to look should be OWASP, which has come out with a list of threats that the community needs to be aware of; this new OWASP list is concentrated on the LLM AI threats.

An important thing to point out here is that nearly half of these threats are the kind of things we see in normal cybersecurity, not AI-related at all. Denial of service, insecure design, and sensitive information disclosure. It is important to point out that the security foundations are still needed. But then there are also some AI-related ones. We have prompt injection, overreliance, and even model theft. Many of the old threats haven't gone away, but some new threats definitely have to pay attention to.

## AI Pipeline System Components
A diagram showing the AI Pipeline split into three main disciplines as well as the flow of tasks and engineering responsibilities from ideation to deployment.
There are various processes and disciplines that are required when building these AI systems. It has been mapped out for a better understanding of this pipeline. The AI pipeline system components are split into three different disciplines. On the far left, we have data engineering, in the middle model engineering, and then finally, application engineering.

This step-by-step flow from ideation all the way out to deployment and users interacting with the application. The flow is split into three different disciplines.

It begins with data engineering and the problem that is being solved. This process is called ideation, and here, is where the requirements are set for the best plan forward.

After ideation, it is time to collect the data from the appropriate data sources. From there, prep that data so that it can be fed for our model to digest. From input-output, we're going to go into model engineering and bring this data all to begin to experiment.

This is where we decide which algorithms to use, trust, and train this code, then evaluate the algorithms through both statistical analysis as well as other types of tests. Then, finally, we're just going to apply that algorithm to the data.

Moving from model engineering to the application side, this is where we are connecting all that insight into the application. This comes in the form of application development as well as the code in that application and plugins that can be added on, which could be developed by third-party developers as well as anyone in the community. And finally, in a sense of maturity, we have monitoring, data privacy, and governance.

Mapped out here are the different threats from the OWASP list, coupled with the engineering discipline in the AI pipeline. There are a lot of data-related threats, engineering, and supply chains that'll be on the data engineering. Then we'll have models, model theft, denial of service, that's concentrated on in the model engineering discipline. Finally, we have prompt injection and a lot of the other vulnerabilities and application engineering.

## Data Science and Engineering
A diagram showing the Data Engineering discipline of the AI Pipeline the flow of tasks and engineering responsibilities as well as specifically mapped out threats.
This phase is important because data is what drives the behavior of the model and the results throughout the whole AI pipeline. Developers should be worried about data quality and sensitive information disclosure. It's important that we keep this intellectual property and model data secure. Next, there are supply chain vulnerabilities, and this really comes into the fact of different tooling.

Many Python packages and a lot of ML packages are open-source and are susceptible to some of these attacks. Finally, there is training data poisoning. This really affects us in the data prep phase, in which any type of corruption will be exponentially more serious the further we go down the pipeline. The focus here is the fact that data is going to determine the behavior of our model, and we need to understand the threats that face it to be able to protect it.

## Model Engineering
A diagram showing the Model Engineering discipline of the AI Pipeline the flow of tasks and engineering responsibilities as well as specifically mapped out threats.
The next phase is model engineering. Model engineering is where we're gaining the insights and real value of this technology. It's the emergent behavior from all the data that we've cleaned and prepped. And a big thing here is model attack prevention.

One attack is a model denial of service. This is really a denial of service; a user cannot access the model for its insight. Next, we have over-reliance. It is more of a broad threat; it's important to put here as you're doing your model analysis and tweaking, and that's really putting too much confidence within the output of this model, which will allow you to build in maybe some controls during this phase. And finally, we have model theft. This can come in a couple of different ways. It can be the actual physical access and stealing of that model. It can come from really harvesting the weights as well as some of that data. This should be built into both protections as well as overall model design.

## Plugin and Application Engineering
A diagram showing the Plugin and Application Engineering discipline of the AI Pipeline, the flow of tasks and engineering responsibilities, as well as specifically mapped out threats.
The final phase of our process involves plugin and application engineering. Here, we've just completed the model. We have all our data, and this is really about user and machine interaction. This is where the Internet, employees, and users will interact with a powerful model. Concentrate on traditional application security. We have a lot of different threats that can model this from insecure plugin design, which is, you know, an architectural choice that only took in some of the data considerations and complexity of the model.

Then, we have insecure output handling. This is due to the fact that this powerful model has a lot of abilities to gain information, but that power can also be used for evil. And then we have a prompt injection, and this is specific to LLMs, and that's when you jailbreak or undermine the system prompt and really gain access to certain parts of the model you shouldn't.

Lastly, we have excessive agency, and this is overconfidence in the model, where it has excessive functionality and more power than it needs for the problem to be solved. This is traditional security the way we see it in most places. There are injection attacks, output handling, insecure design, and a lot of things that, if we take the same principles from other disciplines, can apply here and get good results and security.

## Key Takeaways
1. Different domains have unique properties and need specialized security considerations.
2. Understanding the fundamentals and different disciplines of AI/ML is the first step to security.
3. Terminology provides clarity to complex systems.
