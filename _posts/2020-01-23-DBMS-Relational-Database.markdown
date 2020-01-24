---
layout: "post"
title: "DBMS CH5 Relational Database"
date: 2020-01-23 08:13:59 -0700
author: "mingi hong"
categories: Computer DBMS
permalink: /:categories/:title.html
---

# Relational Terminology
1. Relations - what we have been referring to as simple linear files. Also called tables.
2. Row = record(files) = tuple(relation)
3. Column = field(files) = attribute(relation)
4. Primary key
    - a relation always has a unique
5. Foreign key
    - to connect relation
6. Candidate key
    - if a relation has more than one attribute or minimum group of attributes that represents a way of uniquely identifying the entities, then they are each called a candidate key.
7. One-to-One
8. One-to-Many
9. Many-to-Many (composite relationship)

# Data Integration
1. The relational algebra Join command
    - Cartesian Product
        {comparing every possible combination of two sets, or two relations.}
    - Equijoin
        {a join where two join field values are identical.}
    - Natural join
        {one of the two identical join columns is eliminated.}

**How many foreign keys?**

![CH5.Rent-A-Car](/minglab/assets/CH5.Rent-A-Car.png)

**There are two foreign keys in recording relationship.**

## Exercise
![CH5.minicase1](/minglab/assets/CH5Minicase1.png)
![CH5.minicase2](/minglab/assets/CH5.Minicase2.png)
![CH5.minicase3](/minglab/assets/CH5.Minicase3.png)
![CH5.minicase4](/minglab/assets/CH5.Minicase4.png)

a. Candidate Keys?
- Ship Relation - Ship Number & Ship Name
- Cruise Relation - Cruise Number & Cruise Director
- Port Relation - Port Name & Port Manager
- Visit Relation - Cruise Number Port Name Arrival Date
- Passenger Relation - Passenger Number Passenger Name
- Voyage Relation - Passenger Number Cruise Number Stateroom Number

b. Primary Key
- Ship Number
- Cruise Number Ship Number
- Port Name
- Cruise Number Port Name
- Passenger Number
- Fore - Passenger Number, Cruise Number, Stateroom Number

c. 
- Ship: 0
- Cruise: 1
- Port: 0
- Visit: 2
- Passenger: 0
- Voyage: 2

d.
- Cruise: Ship Number
- Visit: Cruise Number
- Port: Name
- Voyage: Passenger Number, Cruise Number

e.
- For Visit and Voyage relations, both parts of their primary keys are also foreign keys. In the instance of Visit its the port name, country and cruise number where in Voyage it is the cruise number and passenger number. The reason for this is because both of the relations are many to many relationships.

f. Identify the relations that support many-to-many relationships, the primary keys of those relations, and any intersection data.

`VISIT Relation support many-to-many relationship between cruise and port.`
`Primary key for cruise is cruise number and for port is combination of port name and country finally for Visit combination of cruise number and port name and country.`
`VOYAGE Relation support many-to-many relationship between passenger and cruise.`
`Primary key for passenger is passenger number and for cruise is cruise number and for Voyage is combination of passenger number and cruise number.`
`Intersection data are VISIT Relation and VOYAGE Relation.`

g. Using the informal relational command language described in this chapter, write commands to:
1. Retrieve the record for passenger number 473942
    - select record from passenger where passenger number = 473942
2. Retrieve the record for the port of Nassau in the Bahamas.
    - select record from port where port name = Nassau and country = Bahamas
3. List all of the ships built by General Shipbuilding,Inc.
    - select all records from ship where ship builder = General Shipbuilding,Inc
4. List the port name and number of docks of every port in Mexico.
    - select all port name and number of docks from port where country = Mexico.
5. List the name and number of every ship.
    - select all name and number from ship
6. Who was the cruise director on cruise number 38232?
    - select cruise director from cruise where cruise number = 39232
7. What was the gross weight of the ship used for cruise number 39482?
    - select gross weight from ship where Join the cruise and ship using cruise number = 39482
8. List the home address of every passenger on cruise number 17543.
    - select all home address from passenger where Join the cruise and passenger and using cruise number = 17543