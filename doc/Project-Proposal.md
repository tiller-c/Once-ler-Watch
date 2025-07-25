# Project Proposal

### Project Summary & Description
Our app will allow users to visualize global deforestation data from Global Forest Watch. The database consists of seven datasets detailing deforestation trends at various levels (national, sub-national, etc.), along with related carbon data.

### Usefulness
This app usefully addresses a gap in readily available deforestation information by providing users with a concise view of local deforestation trends in the context of global patterns. Existing online resources often lack this localized perspective, making it difficult for users to understand the deforestation situation in their specific area. Our app aims to fill this void while also increasing awareness of this pressing issue.

### Creative Component
Our creative component will be an interactive map, allowing users to click on any location on the map, giving them the deforestation information around that area. Users will also be able to drag a slider below the map to increase or decrease the radius around that location. If the user does not want to use the interactive map to click on a location, the map will display through a color metric the deforestation level of every country. Our app will allow the user to query the most recently searched locations as well as locations with the highest total views. We will implement this by creating an empty dataset to track the recent searches that users can view. When someone views a location, it updates the view count/recency. It will only show the ten most recent searches, and our application will delete old searches from this dataset.

### Features

Our app provides several key features to help users visualize and analyze deforestation data effectively:

#### Location-Based Deforestation Trends  
- Users can **input a location** to view deforestation trends within a specified radius.  
- An **interactive map** allows users to click on any location to get deforestation data for that area.  
- A **slider control** lets users adjust the radius dynamically to expand or narrow the area of analysis.  

#### Global Deforestation Overview  
- If users do not want to click on a specific location, the **map will use a color metric** to represent deforestation levels in every country.  

#### Search History & Popular Locations  
- Users can **query the most recently searched locations** and locations with the **highest total views**.  
- We will maintain a **search history dataset** that tracks recent user queries.  
- When a location is viewed, its **view count and recency** are updated.  
- The app will display only **the ten most recent searches**, removing older searches automatically.  


### Functionality
We will create our app by developing a Shiny app using the R extension in VSCode. Data access will be handled through SQL queries to a Google Cloud database. The app will dynamically update the visualizations based on the user's selected location and radius. We will have multiple tabs with one allowing users to view country and specific location information, and another to view global deforestation data. There will be numerous graphs and data visualizations using either the location information or country information, based on what the user inputs. We will use keyword search functionality to search locations in the world. If a user inputs just “Gorbach”, the search will show all locations with “Gorbach”. Users can “Favorite” locations to easily find them in the future; this will be done using a stored procedure. We will use a transaction to ensure that when recent searches are updated, we maintain the 10 most recent searches correctly without errors. We will use triggers in the transaction to ensure view counts get updated properly. 


### Data Sources and Structure
As for the data we are gathering, we are using the Global Forest Watch’s data as an xmlx file. This dataset contains six distinct datasets with a cardinality of several thousand each and a degree of 26. This will allow us to have plenty of data to aggregate and analyze with multiple relationship types.

### UI Mockup

![UI Mockup](https://github.com/cs411-alawini/sp25-cs411-team109-TeamAwesome/blob/main/doc/UI-mockup.png)
### Work Distribution
For the group distribution, we will all collaborate as needed, but will have sub-specific roles:

- **Jeremy**: Frontend Developer  
- **Akhil**: Backend Developer  
- **Tyler**: Database Manager  
- **Chloe**: Data Analyst  
