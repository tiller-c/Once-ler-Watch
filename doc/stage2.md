# **Explanation**  

The following points explain the reasons why we have selected the entities and attributes, as well as how we have modeled relations between them:  

The Users entity is meant to represent a user of our application. The attributes reflect all the information that is necessary for a user. We also modeled entities that enable our app to track a user’s favorite locations and search history. We modeled them as entities instead of relations with attributes since, otherwise, we would have to modify attributes in the Locations and/or Users entity to relate them to each other, making the schema more complicated. This approach was simpler.  

### **Users to Favorites:**  
A user can have zero to a maximum of 10 favorite locations saved, giving us a (0..10) mapping from Users to Favorites. One entry in the Favorites entity, on the other hand, can map back to exactly one user only.  

### **Users to Search History:**  
Like in Users-Favorites, the search history stores anywhere from 0 to 10 of the users' most recent searches; therefore, we have Users mapping to (0..10) Search Histories. The searchOrderNum attribute shows us how recent the search is (it is a number from 1 to 10, where searchOrderNum = n represents the nth most recent search). A single search history entry can map back to exactly one user.  

### **Location Entity:**  
The Location entity assumes that each location has a unique pairing of one country with one subnation as (location_name, parent_country). To represent countries, we set parent_country to NULL and the location_name to the name of the country. We initially considered having two separate entities for countries and subnations. However, it made more sense to just group them into one entity since this reduces redundancy. We also added the location_id attribute since non-country locations could have duplicate names.  

### **Country Data to Location:**  
In our model, the relationship between Country Data and Location assumes that a location is associated with at most one country, and each country is uniquely identified by its name. Since a country can only exist in one location, and each location corresponds to exactly one country, we modeled the relationship as a one-to-one mapping.  

### **Subnational Data to Location:**  
We recognize that subnational divisions (such as states or provinces) are always tied to a specific country. These divisions do not repeat across countries, so a combination of the subnation name and the country name uniquely identifies them. This dual identification ensures that each subnational entity is distinct and can be referenced within the context of its parent country. Given that users can view the country and subnational data on separate interfaces but need to access them simultaneously, we keep these relationships as one-to-one, ensuring there is no redundancy or conflict in the data.  

### **Favorites to Location:**  
The relationship between Favorites and Location reflects that a favorite location is uniquely associated with a (subnation, country) pair or just a country, all represented as a location_id. Since a favorite can only be one unique subnation or country, we have a one-to-one relationship. However, since we can have multiple locations marked as favorites by many different users, the mapping from Locations to Favorites is (0..*).  

### **Search History to Location:**  
Since the search history can have zero to many locations, we represent this relationship as (0..*), referring to the number of locations possible to appear in the search history. For each location, if it is present in the search history, then we have a (1..1) relation since each location can only have one unique location (similar to how locations act in the Favorites entity).  

---
## **Normalizing the Model:**  
For our designed schema, we have the following relations between entity attributes:  

### **1) Users Entity:**  
This entity is in **BCNF** and **3NF** since all functional dependencies yield every single attribute of the entity as seen below.  
- **userId → Users(email, username, password)**  
- **email → Users(email, userId, password)**  
- **username → Users(userId, email, password)**  

### **2) Favorites Entity:**  
All functional dependencies are trivial for this entity. We can ignore it for normalization.  

### **3) SearchHistory Entity:**  
There is only one functional dependency that yields all attributes, making this **BCNF** and **3NF**.  
- **(userId, searchOrderNum) → (location_id)**  

### **4) Locations Entity:**  
This entity is in **BCNF** and **3NF** since all attributes are yielded.  
- **location_id → (location_name, parent_country, views)**  

### **5) Country Data Entity:**  
This entity is in **BCNF** and **3NF** since the only functional dependency yields every single attribute.  
- **location_id → CountryData(...)**  

### **6) Subnational Data Entity:**  
This entity is in **BCNF** and **3NF** since the only functional dependency yields every single attribute.  
- **location_id → SubnationalData(...)**  

This proves that our **database schema** is in **BCNF** and **3NF**, ensuring that it **minimizes redundancies** as much as possible.

---
# Logical Design  

## Users
    Users(userId: int [PK], 
          email: VARCHAR(255), 
          username: VARCHAR(255), 
          password: VARCHAR(255))

## Locations
    Locations(location_id: int [PK], 
              location_name: VARCHAR(255), 
              parent_country: VARCHAR(255), 
              views: int)

## CountryData
    CountryData(location_id: int [PK, FK to Locations.location_id], 
                country: VARCHAR(255) [FK to Locations.location_name], 
                threshold_cl: int, 
                area_ha: int, 
                tc_loss_ha_2023: int, 
                density_pl: int, 
                primary_forest_extent: int, 
                primary_loss_ha_2023: int, 
                density_cd: int, 
                tree_cover_extent_ha: int, 
                aboveground_carbon_stocks_Mg_C: int, 
                carbon_net_flux__Mg_CO2e: int, 
                carbon_gross_emissions_2023_Mg_CO2e: int, 
                drivers: VARCHAR(255))

## SubnationalData
    SubnationalData(location_id: int [PK, FK to Locations.location_id], 
                    subnation: VARCHAR(255) [FK to Locations.location_name], 
                    threshold_cl: int, 
                    area_ha: int, 
                    tc_loss_ha_2023: int, 
                    density_pl: int, 
                    primary_forest_extent: int, 
                    primary_loss_ha_2023: int, 
                    density_cd: int, 
                    tree_cover_extent_ha: int, 
                    aboveground_carbon_stocks_Mg_C: int, 
                    carbon_net_flux__Mg_CO2e: int, 
                    carbon_gross_emissions_2023_Mg_CO2e: int, 
                    drivers: VARCHAR(255))

## Favorites
    Favorites(userId: int [FK to Users.userId], 
              location_id: int [FK to Locations.location_id])

## SearchHistory
    SearchHistory(userId: int [PK, FK to Users.userId], 
                  location_id: int [FK to Locations.location_id], 
                  searchOrderNum: int [PK])
