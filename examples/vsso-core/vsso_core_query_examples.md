# VSSo core query examples

In this document, we present several query examples that address different competency questions of the VSSo core model.
The [competency questions from VSSo 1.0](https://github.com/w3c/vsso/blob/main/competency-questions.md) were taken as the main reference for comparison purposes.
The queries are written in `SPARQL` language and can be tested on the `vsso_core_data_example.ttl` file available in the working directory.
For that, the example data and the ontology can be imported into a graph database of your preference (with support for RDF data).

- [Q01 - What are the static properties of a given vehicle and what do they express?](#Q01)
- [Q02 - How many attributes (i.e., static properties) does a vehicle have?](#Q02)
- [Q03 - What is the model of this vehicle?](#Q03)
- [Q04 - What is the brand of this car?](#Q04)
- [Q05 - What are the VINs?](#Q05)
- [Q06 - How old is this vehicle?](#Q06)
- [Q07 - What are the dimensions of this vehicle?](#Q07)
- [Q08 - What are the characteristics of this vehicle's chassis?](#Q08)
- [Q09 - What type of fuel does this vehicle need?](#Q09)
- [Q10 - What type of transmission does this vehicle have?](#Q10)
- [Q11 - What are the characteristics of this engine?](#Q11)
- [Q12 - How many doors does this vehicle contain?](#Q12)
- [Q13 - How many seats does this vehicle have?](#Q13)
- [Q14 - On which side is located the steering wheel?](#Q14)
- [Q15 - What properties refer to the steering wheel angle?](#Q15)
- [Q16 - What are the dynamic vehicle properties that can be acted on?](#Q16)
- [Q17 - Which dynamic properties are both observable and actuatable?](#Q17)
- [Q18 - How many dynamic properties does this car contain?](#Q18)
- [Q19 - What properties in this vehicle refer to "Speed"?](#Q19)
- [Q20 - To which part of the vehicle belongs the dynamic property vsso:AccelerationLongitudinal?](#Q20)
- [Which properties refer to a temperature, and in which part of the vehicle?](#Q21)
- [Q22 - What unit type does the signals of type vsso:VehicleYaw use?](#Q22)
- [Q23 - What are the characteristics of the sensor producing the signal “TravelledDistance” in the OBD branch?](#Q23)
- [Q24 - What are the maximum values allowed for all signals from car part “Vehicle”? (not implemented in VSSo 1.0)](#Q24)
- [Q25 - What is the current gear?](#Q25)
- [Q26 - What are the values of all properties representing speed in the vehicle now?](#Q26)
- [Q27 - Which windows are currently open?](#Q27)
- [Q28 - What is the local current temperature on the driver side?](#Q28)

### <a name="Q01"></a> Q01 - What are the static properties of a given vehicle and what do they express?
```SPARQL
PREFIX vsso: <https://www.w3.org/vsso/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT (?svp AS ?StaticVehicleProperty) ?value (?vc AS ?VehicleComponent) WHERE {
    ?v a vsso:Vehicle ;
       vsso:hasVehicleIdentificationVIN "1"^^xsd:string ;  # Assign the VIN of the desired Vehicle
       ?svp ?value .
    ?svp rdfs:subPropertyOf vsso:hasStaticVehicleProperty ;
         vsso:belongsToVehicleComponent ?vc .
}
```

### <a name="Q02"></a> Q02 How many attributes (i.e., static properties) does a vehicle have?
```SPARQL
PREFIX vsso: <https://www.w3.org/vsso/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT (COUNT(?svp) AS ?count_StaticVehicleProperty)  WHERE {
    ?v a vsso:Vehicle ;
       vsso:hasVehicleIdentificationVIN "1"^^xsd:string ;  # Assign the VIN of the desired Vehicle
	   ?svp ?value .
    ?svp rdfs:subPropertyOf vsso:hasStaticVehicleProperty ;
         vsso:belongsToVehicleComponent ?vc .
}
}
```

### <a name="Q03"></a> Q03 - What is the model of this vehicle?
```SPARQL
PREFIX vsso: <https://www.w3.org/vsso/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
SELECT ?model  WHERE {
    ?v a vsso:Vehicle ;
       vsso:hasVehicleIdentificationVIN "1"^^xsd:string ;  # Assign the VIN of the desired Vehicle
	   vsso:hasVehicleIdentificationModel ?model .
}
```

### <a name="Q04"></a> Q04 - What is the brand of this car?
```SPARQL
PREFIX vsso: <https://www.w3.org/vsso/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
SELECT ?brand  WHERE {
    ?v a vsso:Vehicle ;
       vsso:hasVehicleIdentificationVIN "1"^^xsd:string ;  # Assign the VIN of the desired Vehicle
	   vsso:hasVehicleIdentificationBrand ?brand .
}
```

### <a name="Q05"></a> Q05 - What are the VINs?
```SPARQL
PREFIX vsso: <https://www.w3.org/vsso/>
SELECT (?v as ?Vehicle) ?VIN  WHERE {
    ?v a vsso:Vehicle ;
       vsso:hasVehicleIdentificationVIN ?VIN .
} LIMIT 100
```

### <a name="Q06"></a> Q06 - How old is this vehicle?
```SPARQL
PREFIX vsso: <https://www.w3.org/vsso/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
SELECT ?age WHERE {
    ?v rdf:type vsso:Vehicle ;
       vsso:hasVehicleIdentificationVIN "1"^^xsd:string ; # Assign the VIN of the desired Vehicle
       vsso:hasVehicleIdentificationYear ?year .
BIND((2021-?year) AS ?age)}
```

### <a name="Q07"></a> Q07 - What are the dimensions of this vehicle?
```SPARQL
PREFIX vsso: <https://www.w3.org/vsso/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
SELECT ?length ?height ?width  WHERE {
    ?v a vsso:Vehicle ;
       vsso:hasVehicleIdentificationVIN "1"^^xsd:string ; # Assign the VIN of the desired Vehicle
       vsso:hasChassisLength ?length ;
       vsso:hasChassisHeight ?height ;
       vsso:hasChassisWidth ?width .
}
```

### <a name="Q08"></a> Q08 - What are the characteristics of this vehicle's chassis?
```SPARQL
PREFIX vsso: <https://www.w3.org/vsso/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
SELECT * WHERE {
    ?v a vsso:Vehicle ;
       vsso:hasVehicleIdentificationVIN "1"^^xsd:string ; # Assign the VIN of the desired Vehicle
       ?svp ?value .
    ?svp vsso:belongsToVehicleComponent vsso:Chassis .
}
```

### <a name="Q09"></a> Q09 - What type of fuel does this vehicle need?
```SPARQL
PREFIX vsso: <https://www.w3.org/vsso/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
SELECT (?value AS ?FuelType)  WHERE {
    ?v a vsso:Vehicle ;
       vsso:hasVehicleIdentificationVIN "1"^^xsd:string ;  # Assign the VIN of the desired Vehicle
	   vsso:hasFuelSystemFuelType ?value .
}
```

### <a name="Q10"></a> Q10 - What type of transmission does this vehicle have?
```SPARQL
PREFIX vsso: <https://www.w3.org/vsso/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
SELECT (?value AS ?TransmissionType)  WHERE {
    ?v a vsso:Vehicle ;
       vsso:hasVehicleIdentificationVIN "1"^^xsd:string ;  # Assign the VIN of the desired Vehicle
	   vsso:hasTransmissionType ?value .
}
```

### <a name="Q11"></a> Q11 - What are the characteristics of this engine?
```SPARQL
PREFIX vsso: <https://www.w3.org/vsso/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT (?svp AS ?StaticVehicleProperty) ?value WHERE {
    ?v a vsso:Vehicle ;
       vsso:hasVehicleIdentificationVIN "1" ;  # Assign the VIN of the desired Vehicle
       ?svp ?value .
    ?svp rdfs:subPropertyOf vsso:hasStaticVehicleProperty ;
         vsso:belongsToVehicleComponent vsso:PowerSourceCombustionEngine .  
}
```

### <a name="Q12"></a> Q12 - How many doors does this vehicle contain?
```SPARQL
PREFIX vsso: <https://www.w3.org/vsso/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
SELECT ?CabinDoorCount  WHERE {
    ?v a vsso:Vehicle ;
       vsso:hasVehicleIdentificationVIN "1"^^xsd:string ;  # Assign the VIN of the desired Vehicle
	   vsso:hasCabinDoorCount ?CabinDoorCount .
}
```

### <a name="Q13"></a> Q13 - How many seats does this vehicle have?
```SPARQL
PREFIX vsso: <https://www.w3.org/vsso/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
SELECT ?vehicleSeatingCapacity  WHERE {
    ?v a vsso:Vehicle ;
       vsso:hasVehicleIdentificationVIN "1"^^xsd:string ;  # Assign the VIN of the desired Vehicle
	   vsso:hasVehicleIdentificationvehicleSeatingCapacity ?vehicleSeatingCapacity .
}
```

### <a name="Q14"></a> Q14 - On which side is located the steering wheel?
```SPARQL
PREFIX vsso: <https://www.w3.org/vsso/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
SELECT ?SteeringWheelPosition  WHERE {
    ?v a vsso:Vehicle ;
       vsso:hasVehicleIdentificationVIN "1"^^xsd:string ;  # Assign the VIN of the desired Vehicle
	   vsso:hasSteeringWheelPosition ?SteeringWheelPosition .
}
```

### <a name="Q15"></a> Q15 - What properties refer to the steering wheel angle?
```SPARQL
SELECT * WHERE {
    ?s ?p ?o . FILTER contains(lcase(?o),"angle"@en)
}
```

### <a name="Q16"></a> Q16 - What are the dynamic vehicle properties that can be acted on?
```SPARQL
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX vsso: <https://www.w3.org/vsso/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT (?dvp AS ?DynamicVehicleProperty) ?class WHERE {
    ?v a vsso:Vehicle ;
       vsso:hasVehicleIdentificationVIN "1"^^xsd:string ;  # Assign the VIN of the desired Vehicle
	   vsso:hasVehicleProperty ?dvp .
    ?dvp a ?class .
    ?class rdfs:subClassOf vsso:ActuatableVehicleProperty .
}
```

### <a name="Q17"></a> Q17 - Which dynamic properties are both observable and actuatable?
```SPARQL
PREFIX vsso: <https://www.w3.org/vsso/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT (?dvp AS ?DynamicVehicleProperty) ?class WHERE {
    ?v a vsso:Vehicle ;
       vsso:hasVehicleIdentificationVIN "1"^^xsd:string ;  # Assign the VIN of the desired Vehicle
	   vsso:hasDynamicVehicleProperty ?dvp .
    ?dvp a ?class .
    ?class rdfs:subClassOf vsso:ActuatableVehicleProperty ,
            			   vsso:ObservableVehicleProperty .
}
```

### <a name="Q18"></a> Q18 - How many dynamic properties does this car contain?
```SPARQL
PREFIX vsso: <https://www.w3.org/vsso/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
SELECT (COUNT(?dvp) AS ?count_DynamicVehicleProperty) WHERE {
    ?v a vsso:Vehicle ;
       vsso:hasVehicleIdentificationVIN "1"^^xsd:string ;  # Assign the VIN of the desired Vehicle
	   vsso:hasDynamicVehicleProperty ?dvp .
}
```

### <a name="Q19"></a> Q19 - What properties in this vehicle refer to "Speed"?
```SPARQL
PREFIX vsso: <https://www.w3.org/vsso/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT ?dvp ?class WHERE {
    ?v a vsso:Vehicle ;
       vsso:hasVehicleIdentificationVIN "1"^^xsd:string ;  # Assign the VIN of the desired Vehicle
	   vsso:hasDynamicVehicleProperty ?dvp .
    ?dvp a ?class .
    ?class rdfs:label "Speed"@en .
}
```

### <a name="Q20"></a> Q20 - To which part of the vehicle belongs the dynamic property vsso:AccelerationLongitudinal?
```SPARQL
PREFIX vsso: <https://www.w3.org/vsso/>
select (?vc AS ?VehicleComponent) ?source where {
	vsso:AccelerationLongitudinal vsso:belongsToVehicleComponent ?vc .
    ?vc vsso:partOf ?source .
}
```

### <a name="Q21"></a> Q21 - Which properties refer to a temperature, and in which part of the vehicle?
```SPARQL
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX vsso: <https://www.w3.org/vsso/>
SELECT DISTINCT * where {
  ?dvp rdfs:label ?o . FILTER contains(lcase(?o),"temperature")
  ?dvp vsso:belongsToVehicleComponent ?vc .
  ?vc vsso:partOf ?partOf .
}
```

### <a name="Q22"></a> Q22 - What unit type does the signals of type vsso:VehicleYaw use?
```SPARQL
SELECT * WHERE {
    vsso:AngularVelocityYaw schema:rangeIncludes ?o .
}
```

### <a name="Q23"></a> Q23 - What are the characteristics of the sensor producing the signal “TravelledDistance” in the OBD branch?
```SPARQL
PREFIX vsso: <https://www.w3.org/vsso/>
select * where {
	vsso:TravelledDistance ?p ?o .
}
```

### <a name="Q24"></a> Q24 - What are the maximum values allowed for all signals from car part “Vehicle”? (not implemented in VSSo 1.0)
```SPARQL

```

### <a name="Q25"></a> Q25 - What is the current gear?
```SPARQL
PREFIX vsso: <https://www.w3.org/vsso/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
select (?dvp AS ?DynamicVehicleProperty) ?value where {
    ?v a vsso:Vehicle ;
       vsso:hasVehicleIdentificationVIN "1"^^xsd:string ;  # Assign the VIN of the desired Vehicle
       vsso:hasDynamicVehicleProperty ?dvp .
    ?dvp a vsso:TransmissionGear ;
         vsso:holdsState ?value .
}
```

### <a name="Q26"></a> Q26 - What are the values of all properties representing speed in the vehicle now?
```SPARQL
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX vsso: <https://www.w3.org/vsso/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
select ?dvp ?value where {
    ?v a vsso:Vehicle ;
       vsso:hasVehicleIdentificationVIN "1"^^xsd:string ;  # Assign the VIN of the desired Vehicle
       vsso:hasDynamicVehicleProperty ?dvp .
    ?dvp vsso:holdsState ?value ;
         a ?class .
    ?class rdfs:label ?label . FILTER contains(lcase(?label),"speed")
}
```

### <a name="Q27"></a> Q27 - Which windows are currently open? <a name="Q27"></a>
```SPARQL
PREFIX vsso: <https://www.w3.org/vsso/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
select (?dvp AS ?DynamicVehicleProperty) ?value where {
    ?v a vsso:Vehicle ;
       vsso:hasVehicleIdentificationVIN "1"^^xsd:string ;  # Assign the VIN of the desired Vehicle
       vsso:hasDynamicVehicleProperty ?dvp .
    ?dvp a vsso:WindowisOpen ;
         vsso:holdsState ?value
}
```

### <a name="Q28"></a> Q28 - What is the local current temperature on the driver side?
```SPARQL
PREFIX vsso: <https://www.w3.org/vsso/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
select (?dvp AS ?DynamicVehicleProperty) ?value where {
    ?v a vsso:Vehicle ;
       vsso:hasVehicleIdentificationVIN "1"^^xsd:string ;  # Assign the VIN of the desired Vehicle
       vsso:hasDynamicVehicleProperty ?dvp .
    ?dvp a vsso:AmbientAirTemperature ;
         vsso:holdsState ?value .
}
```
