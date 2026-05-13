I need an SAP Integration Flow (iFlow) which does the following:
Just connect a Start Message step with and End Message step.

Generate the iFlow as folder with a suitable name and such that it contains all folders and files in the correct format such that in can be imported into SAP Integration Suite as iFlow. Make sure the inside the zip file the following folder structure is applied:

iFlowName/
├── META-INF/
│   └── MANIFEST.MF
├── src/
│   └── main/
│       └── resources/
│           ├── scenarioflows/
│           │   └── integrationflow/
│           │       └── iFlowName.iflw
│ 			├── parameters.prop
│ 			└── parameters.propdef
├─── .project
└─── metainfo.prop

Further, the MANIFEST.MF file must contain at least the following fields (the already present values can be kept):
Manifest-Version: 1.0
Bundle-ManifestVersion: 2
Bundle-Name: 
Bundle-SymbolicName: 
Bundle-Version: 1.0.0
SAP-BundleType: IntegrationFlow
SAP-NodeType: IFLMAP
SAP-RuntimeProfile: iflmap

The .iflw file should follow this xsd schema:
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema
    xmlns:xs="http://www.w3.org/2001/XMLSchema"
    xmlns:bpmn2="http://www.omg.org/spec/BPMN/20100524/MODEL"
    xmlns:bpmndi="http://www.omg.org/spec/BPMN/20100524/DI"
    xmlns:dc="http://www.omg.org/spec/DD/20100524/DC"
    xmlns:di="http://www.omg.org/spec/DD/20100524/DI"
    xmlns:ifl="http:///com.sap.ifl.model/Ifl.xsd"
    targetNamespace="http://www.omg.org/spec/BPMN/20100524/MODEL"
    elementFormDefault="qualified"
    attributeFormDefault="unqualified">

    <xs:import namespace="http://www.omg.org/spec/BPMN/20100524/DI"/>
    <xs:import namespace="http://www.omg.org/spec/DD/20100524/DC"/>
    <xs:import namespace="http://www.omg.org/spec/DD/20100524/DI"/>
    <xs:import namespace="http:///com.sap.ifl.model/Ifl.xsd"/>

    <xs:element name="definitions" type="bpmn2:Definitions"/>

    <xs:complexType name="Definitions">
        <xs:sequence>
            <xs:element name="collaboration" type="bpmn2:Collaboration"/>
            <xs:element ref="bpmn2:process"/>
            <xs:element ref="bpmndi:BPMNDiagram"/>
        </xs:sequence>
        <xs:attribute name="id" type="xs:string" use="required"/>
    </xs:complexType>

    <xs:element name="process" type="bpmn2:Process"/>

    <xs:complexType name="ExtensionElements">
        <xs:sequence>
            <xs:element ref="ifl:property" minOccurs="0" maxOccurs="unbounded"/>
        </xs:sequence>
    </xs:complexType>

    <xs:complexType name="Property">
        <xs:sequence>
            <xs:element name="key" type="xs:string"/>
            <xs:element name="value" type="xs:string" minOccurs="0"/>
        </xs:sequence>
    </xs:complexType>

    <xs:complexType name="Collaboration">
        <xs:sequence>
            <xs:element name="extensionElements" type="bpmn2:ExtensionElements" minOccurs="0"/>
            <xs:element name="participant" type="bpmn2:Participant" maxOccurs="unbounded"/>
        </xs:sequence>
        <xs:attribute name="id" type="xs:string" use="required"/>
        <xs:attribute name="name" type="xs:string" use="optional"/>
    </xs:complexType>

    <xs:complexType name="Participant">
        <xs:sequence>
            <xs:element name="extensionElements" type="bpmn2:ExtensionElements" minOccurs="0"/>
        </xs:sequence>
        <xs:attribute ref="ifl:type" use="optional"/>
        <xs:attribute name="id" type="xs:string" use="required"/>
        <xs:attribute name="name" type="xs:string" use="optional"/>
        <xs:attribute name="processRef" type="xs:string" use="required"/>
    </xs:complexType>

    <xs:complexType name="Process">
        <xs:sequence>
            <xs:element name="extensionElements" type="bpmn2:ExtensionElements" minOccurs="0"/>
            <xs:element name="endEvent" type="bpmn2:EndEvent" minOccurs="0" maxOccurs="unbounded"/>
            <xs:element name="startEvent" type="bpmn2:StartEvent" minOccurs="0" maxOccurs="unbounded"/>
            <xs:element name="sequenceFlow" type="bpmn2:SequenceFlow" minOccurs="0" maxOccurs="unbounded"/>
        </xs:sequence>
        <xs:attribute name="id" type="xs:string" use="required"/>
        <xs:attribute name="name" type="xs:string" use="optional"/>
    </xs:complexType>

    <xs:complexType name="StartEvent">
        <xs:sequence>
            <xs:element name="extensionElements" type="bpmn2:ExtensionElements" minOccurs="0"/>
            <xs:element name="outgoing" type="xs:string" minOccurs="0" maxOccurs="unbounded"/>
            <xs:element name="messageEventDefinition" type="xs:anyType" minOccurs="0"/>
        </xs:sequence>
        <xs:attribute name="id" type="xs:string" use="required"/>
        <xs:attribute name="name" type="xs:string" use="optional"/>
    </xs:complexType>

    <xs:complexType name="EndEvent">
        <xs:sequence>
            <xs:element name="extensionElements" type="bpmn2:ExtensionElements" minOccurs="0"/>
            <xs:element name="incoming" type="xs:string" minOccurs="0" maxOccurs="unbounded"/>
            <xs:element name="messageEventDefinition" type="xs:anyType" minOccurs="0"/>
        </xs:sequence>
        <xs:attribute name="id" type="xs:string" use="required"/>
        <xs:attribute name="name" type="xs:string" use="optional"/>
    </xs:complexType>

    <xs:complexType name="SequenceFlow">
        <xs:attribute name="id" type="xs:string" use="required"/>
        <xs:attribute name="sourceRef" type="xs:string" use="required"/>
        <xs:attribute name="targetRef" type="xs:string" use="required"/>
    </xs:complexType>

</xs:schema>

It might not be complete in the sense that other complexTypes can be added for certain iFlow steps. But the rought structure around pmn2:definitions, bpmn2:collaboration, bpmn2:process, pmndi:BPMNDiagram must be applied.

Push the resulting folder to the develop branch of the repo PhilipWSov/iFlowGen into the folder iFlowGenerations.
