# Semantic Machine Capability and Skill Model

<p align="center">
  <img src="https://github.com/aljoshakoecher/machine-capability-model/blob/documentation/images/images/Capability-ODPs.png?raw=true" alt="Overview of the ontology design patterns used" width="500"/>
</p>
<p align="center">
  <b>Figure 1:</b> Overview of the ODPs used in this capability and skill model.
</p>

## Overview
This repository contains an ontology that can be used to create a semantic description of machines, their capabilities and their executable skills. This ontology combines a variety of different so-called ontology design patterns (ODPs) that are all based on industry standards. As shown in Figure 2, the OPDs / standards included in this model are:
* **VDI 3682**: Defines a simple way to model processes with their in- and outputs
* **VDI 2860**: Contains a (German) taxonomy of manufacturing processes
* **DIN 8580**: Contains a (German) taxonomy of handling processes
* **VDI 2206**: Defines basic terms to describe machines and the way they consist of modules and components and have interfaces with other machines. 
* **DIN EN 61360**: Can be used to model attributes in a formal way
* **ISA 88**: Defines a state machine which is widely used in automation (e.g. for packaging machines)
* **Web Application Description Language**: A lightweight XML-based W3C submission to describe RESTful web services. We created an ontology for this XML schema.
* **OPC UA**: Platform independent technologoy to describe and communicate data and methods. Used for data access and method calls and the de-facto standard for a modern communication architecture in automation

All these ODPs are maintained in separate Github repositories. You can find an archived overview with links to all ODP repos at [Industry Standard ODP-Repository](https://github.com/hsu-aut/Industrial-Standard-Ontology-Design-Patterns). Documentation to every ODP is provided in its corresponding repository.

## Alignment Ontology
This machine capability and skill ontology can be seen as an alignment ontology that connects the aforementioned standard ODPs. Figure 2 gives an overview on how these ODPs were connected to get a model that can be used to describe machines, their capabilities and executable skills:

![Capability Ontology as an alignment ontology of different standards](https://github.com/aljoshakoecher/machine-capability-model/blob/documentation/images/images/AlignmentOntology.png?raw=true)
<p align="center">
  <b>Figure 2:</b> Alignment Ontology that combines various industry standards.
</p>

## Documentation
The ontology consists of four main parts:
* Machine structure: Defines a machine and its components as well as interfaces to other machines
* Abstract capabilities: Processes that a machine is able to execute, modelled with their in- and output products.
* Executable skills: A description of a technical implementation that can be used to execute a capability. Currently, we support RESTful web services and OPC UA.
* Properties: A meta-model that allows to define properties and express statements over properties.

These three parts are explained in the following sections.

### Modeling machine structure
Each machine should be modelled as an individual of the class `VDI3682:TechnicalResource` which contains the subclasses `VDI:2206:System` and `VDI2206:Module`. Choose any of the two depending on the complexity of the machine you're describing. Additional elements of VDI2206 might be used to further specify your machine (not shown in Fig. 1). You could e.g. model the components of your machine or you could model mechanical, electrical or information interfaces to other machines. Have a look at the [VDI 2206 in our Industry Standard ODP-Repository](https://github.com/hsu-aut/Industrial-Standard-Ontology-Design-Patterns/VDI2206) for further info.

### Modeling abstract capabilities
Abstract capabilities are modelled as processes according to VDI 3682. At the most basic level, you can simply create an individual of type `Cap:Capability` which is an alias for `VDI3682:Process`. In order to give more information about the kind of process you're modelling, it's better to use one of the many subclasses of `Cap:Capability`. These subclasses are handling operations (according to VDI 2860) and manufacturing operations (according to DIN 8580). You could for example model a milling process as an individual of the class `DIN8580:Fraesen` (=German word for milling). Unfortunately, DIN 8580 doesn't provide an English translation, so all its terms are in German. We might publish an unofficial translation to our Industry Standard Repo soon. </br>
After modelling all capabilities of your machine, use the ObjectProperty `Cap:hasCapability` to connect your machine with its capabilities.

### Modeling executable skills
Skills are a description of the interface technology that can be used to execute automated processes. We currently support OPC UA and RESTful webservices to execute skills. 
A skill consists of a model of a state machine (we make use of the ISA 88 state machine) and of the interface methods that can be used to interact with this state machine (either via REST or OPC UA methods).<br>
Please note that manually modelling a skill is cumbersome and error-prone. We strongly suggest that you have a look at [SkillUp](https://github.com/aljoshakoecher/skill-up), a Java-framework that automatically creates valid skills for you based on plain Java classes with some annotations. With *SkillUp*, you can program machine skills like you may be used to do it from modern frameworks like Spring, Angular or NestJS. You don't need to worry about a state machine, about REST / OPC UA and about the ontology. You just have to program the behavior of your skill like you normally would and *SkillUp* does the rest for you. <br>
If you can't use *SkillUp* or you don't want to, have a look at [LiOn](https://github.com/hsu-aut/lion), which is a web based tool to support a user in creating ontological models (i.e. the A-Box for given T-Boxes). With *LiOn*, you can, for example, create a complete state machine model with just a few clicks.<br><br>

If you really want to model a skill complete by hand and from scratch, you need to create an individual of one of the two classes `Cap:RestSkill` or `Cap:OpcUaSkill`. This individual has to be connected with the capbility it belongs to via `Cap:isExecutableVia`. You then have to model individuals for all states and transitions of an ISA88 state machine. Furthermore, depending on the technology used in your implementation, you have to model REST resources or OPC UA methods and connect them with the corresponding transitions. 
Please have a look at the examples for more info.

### Properties
Coming soon...

## Examples
Examples are coming soon...

## How to cite
This ontology has been published in a conference paper. You can find this paper on [ResearchGate](https://www.researchgate.net/publication/342260488_A_Formal_Capability_and_Skill_Model_for_Use_in_Plug_and_Produce_Scenarios). Feel free to contact us via ResearchGate in case you have questions about the paper.
In case you want to use this ontology in your own research, please this paper cite as:
```
A. Kocher, C. Hildebrandt, L. M. Vieira da Silva, and A. Fay, “A formal capability  and  skill  model  for  use  in  plug  and  produce  scenarios  (accepted for publication),” in Proceedings, 2020 25th IEEE InternationalConference on Emerging Technologies and Factory Automation (ETFA).Piscataway, NJ: IEEE, 2020.
```

