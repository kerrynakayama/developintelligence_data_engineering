# Lab 01 
## Looking at transaction data & theroizing the ETL 

### What is an ER diagram?

![Alt Text](https://media.giphy.com/media/xUNd9DL4a9bJHnlI8U/giphy.gif) 


An Entity Relationship (ER) Diagram is a type of flowchart that illustrates how “entities” such as people, objects or concepts relate to each other within a system. ER Diagrams are most often used to design or debug relational databases in the fields of software engineering, business information systems, education and research. Also known as ERDs or ER Models, they use a defined set of symbols such as rectangles, diamonds, ovals and connecting lines to depict the interconnectedness of entities, relationships and their attributes.



![Image of creating a no cluster table with order](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_01/LAB_01/IMAGES/erd.jpg)
![Image of creating a no cluster table with order](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_01/LAB_01/IMAGES/erd2.png)
![Image of creating a no cluster table with order](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_01/LAB_01/IMAGES/erd3.png)


## What is an ERD good for?

### Database design: 
ER diagrams are used to model and design relational databases, in terms of logic and business rules (in a logical data model) and in terms of the specific technology to be implemented (in a physical data model.) In software engineering, an ER diagram is often an initial step in determining requirements for an information systems project. It’s also later used to model a particular database or databases. A relational database has an equivalent relational table and can potentially be expressed that way as needed.

### Business process re-engineering (BPR): 
ER diagrams help in analyzing databases used in business process re-engineering and in modeling a new database setup.

### Business information systems: 
The diagrams are used to design or analyze relational databases used in business processes. Any business process that uses fielded data involving entities, actions and interplay can potentially benefit from a relational database. It can streamline processes, uncover information more easily and improve results.

## What are the components of an ERD?

### Entity

A definable thing—such as a person, object, concept or event—that can have data stored about it. Think of entities as nouns. Examples: a customer, student, car or product. Typically shown as a rectangle.

### Relationship

How entities act upon each other or are associated with each other. Think of relationships as verbs. For example, the named student might register for a course. The two entities would be the student and the course, and the relationship depicted is the act of enrolling, connecting the two entities in that way. Relationships are typically shown as diamonds or labels directly on the connecting lines.

### Fact tables

### Dim tables

### What are some Questions that need to be answered by the DB?


## Lab Time 

Break into groups of 3-5 people 
Download the data 

![Image of creating a no cluster table with order](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_01/LAB_01/IMAGES/Screen%20Shot%202020-07-05%20at%203.53.37%20PM.png)

- Figure out what are your core questions that will need to be answered by your data

- The data is a streaming set of data that comes in every 15 minutes 

- What needs to be shown live vs can be batched?

- What data should be apart of the FACT tables and what should be put in the DIM tables?

- Is there anything wrong with the data?

- Should there be any QA of the data?

- Are there any validations that need to be performed?

- Should you Normalize the data? 

- Do we have any special data like semi-structure?  

- Does it need to be flattened?

# Present a basic plan of what you plan to do with this data set

*hint - we might revisit this data set later* 

![Image of creating a no cluster table with order](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_01/LAB_01/IMAGES/Screen%20Shot%202020-07-02%20at%209.46.21%20AM.png)
![Image of creating a no cluster table with order](https://github.com/kerrynakayama/developintelligence_data_engineering/blob/master/Day_01/LAB_01/IMAGES/Screen%20Shot%202020-07-05%20at%203.53.37%20PM.png)





