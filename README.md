# CaSkMan - An Ontology for Capabilities and Skill of Manufacturing Machines

## Introduction
In the context of automated production, Capabilities and Skills are terms that are used to refer to machine functions. A *Capability* is an implementation-independent specification of a function to achieve an effect in the physical or virtual world. A *Capability* may be implemented by one or more *Skills*. Skills are encapsulated implementations with a well-defined invocation interface (e.g., using OPC UA).
Consider a Capability as a machine-interpretable description of a function. Such a description typically features *Properties* and constraints on these properties.

For the terminology around capabilities and skills, an abstract *CSS Reference Model* was defined in a [Plattform Industrie 4.0 Whitepaper](https://www.plattform-i40.de/IP/Redaktion/EN/Downloads/Publikation/CapabilitiesSkillsServices.html) and additionally published in a [scientific publication](https://www.degruyter.com/document/doi/10.1515/auto-2022-0117/html).

## Ontology Architecture
<p align="center">
<img src="https://github.com/aljoshakoecher/caskman/blob/documentation/images/images/architecture_marker-caskman.jpg?raw=true" width="800" title="CaSkMan Architecture">
</p>
<p align="center">
  <b>Figure 1:</b>Ontology architecture of the ontologies CSS, CaSk and CaSkMan.
</p>

CaSkMan is part of a three-level ontology architecture. The CSS reference model is implemented by the abstract [CSS Ontology](https://github.com/hsu-aut/css-ontology). The CSS ontology covers all terms and relations of the reference model. But these elements are too generic to model specific capabilities and skills. The CSS ontology does not provide possibilities to model capability parameters in the detail or to model skill interfaces in a specific technology. An extension is provided by the [CaSk ontology](https://github.com/hsu-aut/cask), which can be seen as an intermediate level between the generic CSS ontology and CaSkMan. CaSk extends the CSS ontology and at the same time maintains extensibility for domain-specific ontologies, such as CaSkMan.

The CaSkMan ontology can be used to model capabilities and skills in manufacturing. It extends CaSk by standards such as DIN 8580 (manufacturing operations), VDI 2860 (handling operations) as well as WADL and OPC UA to model skills with interfaces based on web services as well as OPC UA (see details below).
With the [RoboCaSk ontology](https://github.com/Miguel2617/robocap) there is another domain-specific ontology that can be used for capabilities and skills of autonomous systems that can work collaboratively to achieve a common goal. It extends CaSk by additional elements to represent such systems and thei skill interfaces.


## Overview of CaSkMan
<p align="center">
  <img src="https://github.com/aljoshakoecher/machine-capability-model/blob/documentation/images/images/Capability-ODPs.png?raw=true" alt="Overview of the ontology design patterns used" width="500"/>
</p>
<p align="center">
  <b>Figure 2:</b> Overview of the ODPs used in CaSk and CaSkMan.
</p>

The CaSkMan ontology extends the CaSk ontology and both import and combine a variety of different so-called Ontology Design Patterns (ODPs) that are all based on industry standards. Figure 2 shows the OPDs / standards that are included directly in CaSkMan and transitively through the import of CaSk. These ODPs are:
* **VDI 3682**: Defines a simple way to model processes with their in- and outputs
* **VDI 2860**: Contains a (German) taxonomy of manufacturing processes
* **DIN 8580**: Contains a (German) taxonomy of handling processes
* **VDI 2206**: Defines basic terms to describe machines and the way they consist of modules and components and have interfaces with other machines. 
* **DIN EN 61360**: Can be used to model attributes in a formal way
* **ISA 88**: Defines a state machine which is widely used in automation (e.g. for packaging machines)
* **Web Application Description Language**: A lightweight XML-based W3C submission to describe RESTful web services. We created an ontology for this XML schema.
* **OPC UA**: Platform independent technologoy to describe and communicate data and methods. Used for data access and method calls and the de-facto standard for a modern communication architecture in automation

All these ODPs are maintained in separate Github repositories. You can find an archived overview with links to all ODP repos at [Industry Standard ODP-Repository](https://github.com/hsu-aut/Industrial-Standard-Ontology-Design-Patterns). A detailed documentation to every ODP is provided in its corresponding repository.

## Alignment Ontology
CaSkMan can be seen as an alignment ontology that connects the aforementioned standard ODPs. Figure 3 gives an overview on how these ODPs were connected to get one coherent model that can be used to describe capabilities and executable skills of manufacturing machines. Please note that the details of OpcUa and RESTful interfaces were omitted. Take a look at the examples or the Github Repositories of the [WADL](https://github.com/hsu-aut/IndustrialStandard-ODP-WADL) and [OPC UA](https://github.com/hsu-aut/IndustrialStandard-ODP-OPC-UA) ODP

![Capability Ontology as an alignment ontology of different standards](https://github.com/aljoshakoecher/machine-capability-model/blob/documentation/images/images/caskman-alignment.png?raw=true)
<p align="center">
  <b>Figure 3:</b> Alignment Ontology that combines various industry standards.
</p>

If we look at CaSk and CaSkMan as one alignment ontology, this ontology consists of four main parts:
* Machine structure: Defines a machine and its components as well as interfaces to other machines
* Abstract capabilities: Processes that a machine is able to execute, modelled with their in- and output products.
* Executable skills: A description of a technical implementation that can be used to execute a capability. Currently, we support RESTful web services and OPC UA.
* Properties: A meta-model that allows to define properties and express statements over properties.

These three parts are explained in the following sections.

### Modeling machine structure
Each machine should be modelled as an individual of the class `VDI3682:TechnicalResource` which contains the subclasses `VDI:2206:System` and `VDI2206:Module`. Choose any of the two depending on the complexity of the machine you're describing. Additional elements of VDI2206 might be used to further specify your machine (not shown in Fig. 1). You could e.g. model the components of your machine or you could model mechanical, electrical or information interfaces to other machines. Have a look at the [VDI 2206 in our Industry Standard ODP-Repository](https://github.com/hsu-aut/Industrial-Standard-Ontology-Design-Patterns/VDI2206) for further info.

### Modeling abstract capabilities
Abstract capabilities are modelled as processes according to VDI 3682. At the most basic level, you can simply create an individual of type `Cap:Capability` which is an alias for `VDI3682:Process`. In order to give more information about the kind of process you're modelling, it's better to use one of the many subclasses of `Cap:Capability`. These subclasses are handling operations (according to VDI 2860) and manufacturing operations (according to DIN 8580). You could for example model a milling process as an individual of the class `DIN8580:Fraesen` (=German word for milling). Unfortunately, DIN 8580 doesn't provide an English translation, so all its terms are in German</br>
After modelling all capabilities of your machine, use the ObjectProperty `CSS:providesCapability` defined in the [CSS ontology](https://github.com/hsu-aut/css-ontology) to connect your machine with its capabilities.

### Modeling executable skills
Skills are an encapsulated implementation of an automated function. Every skill comes with a description of its skill interface, the interface technology that can be used to execute automated functions. CaSkMan currently supports OPC UA and RESTful webservices to execute skills. 
A skill consists of a model of a state machine (we make use of the ISA 88 state machine) and of the interface methods that can be used to interact with this state machine (either via REST or OPC UA methods).<br>
Please note that manually modelling a skill is cumbersome and error-prone. We strongly suggest that you have a look at [SkillUp](https://github.com/aljoshakoecher/skill-up), a Java-framework that automatically creates valid skills for you based on plain Java classes with some annotations. With *SkillUp*, you can program machine skills like you may be used to do it from modern frameworks like Spring, Angular or NestJS. You don't need to worry about a state machine, about REST / OPC UA and about the ontology. You just have to program the behavior of your skill like you normally would and *SkillUp* does the rest for you. <br>
In case you cannot use Java to implement your skills, you may have a look at [PLC2Skill](https://github.com/aljoshakoecher/plc2skill). PLC2Skill is an automated mapping approach to transfer conventional PLC code according to IEC 61131 into the CaSkMan ontology. And if you already have a Module Type Package (MTP) and want to convert it into a CaSkMan model, you can use [MTP2Skill](https://github.com/hsu-aut/mtp2skill).

If you can't use any of these automated approaches to skill implementation, have a look at [LiOn](https://github.com/hsu-aut/lion), which is a web based tool to support a user in creating ontological models (i.e. the A-Box for given T-Boxes). With *LiOn*, you can, for example, create a complete state machine model with just a few clicks.<br><br>

If you really want to model a skill complete by hand and from scratch, you need to create an individual of the class `CSS:Skill` (or one of its subclasses). A skill has to be connected to a skill interface, which can be either a `CaSkMan:RestSkillInterface` or one of the two subclasses of `Cap:OpcUaSkillInterface`. Every capability is connected to the skills that implement it via `CSS:isRealizedBy`. 
You then have to model individuals for all states and transitions of an ISA88 state machine. This state machine has to be exposed by the skill interface. Furthermore, depending on the technology used in your implementation, you have to model REST resources or OPC UA methods and connect them with the corresponding transitions. And of course you can also model skill parameters. As you may see by now it's a lot of effort to create a CaSkMan representation for your machine manually. We therefore encourage you again to try out our automated methods. Let us know if you need help with SkillUp, PLC2Skill or MTP2Skill.

If you want to see a detailed model of a skill, check out the examples directory.

### Properties
In addition to resources, capabilities and skills, properties can be modeled according to IEC 61360. For every property, a unique type individual can be defined along with all type-related information (e.g., name, ID, unit of measure). Instances can then be created for this type with various expression goals, i.e., as a requirement, an actual value or an assurance. A detailed documentation of the IEC 61360 ODP can be found in the according [Github Repo](https://github.com/hsu-aut/IndustrialStandard-ODP-DINEN61360).

## Examples
In the `examples` directory, you can find examples of a module, a capability as well as a skill.

## How to cite
This ontology has been published in a conference paper. You can find this paper on [ResearchGate](https://www.researchgate.net/publication/342260488_A_Formal_Capability_and_Skill_Model_for_Use_in_Plug_and_Produce_Scenarios). Feel free to contact us via ResearchGate in case you have questions about the paper.
In case you want to use this ontology in your own research, please this paper cite as:
```
A. Köcher, C. Hildebrandt, L. M. Vieira da Silva, and A. Fay, “A formal capability  and  skill  model  for  use  in  plug  and  produce  scenarios” in Proceedings, 2020 25th IEEE InternationalConference on Emerging Technologies and Factory Automation (ETFA).Piscataway, NJ: IEEE, 2020.
```

