# Web Crawler with Neo4j Integration

# Overview
This project implements a powerful web crawler that fetches and processes web pages, storing the data in a Neo4j database for analysis and visualization. The crawler captures metadata like page titles and links while enabling graph-based insights such as PageRank, link density, and connectivity visualization.

# Features

# Crawling Capabilities

    1) Fetches web pages starting from a given URL up to a specified depth. 

# Neo4j Integration

    1) Stores pages and links as nodes and relationships in a graph database.

# Graph Analytics

    1) PageRank calculation: Determines the importance of each page.

    2) Identification of highly connected pages: Highlights hubs in the graph.

    3) Detection of orphan pages and dead ends: Identifies pages without connections.

# Visualization

    1) Graph-based visualization of page relationships, scaled by outgoing link counts.

# Requirements

# Software

    1) Java: JDK 11 or later.

    2) Neo4j: Installed and running on localhost with Bolt protocol enabled.

# Dependencies

    1) Neo4j Java Driver: For connecting and interacting with Neo4j.

    2) Jsoup: For HTML parsing.

    3) Apache Log4j: For logging application activities.

# Setup

# 1. Clone the Repository
          git clone https://github.com/yourusername/web-crawler-neo4j.git
          cd web-crawler-neo4j

# 2. Configure Neo4j

       1) Start the Neo4j server.
       2) Set credentials and update the following in neo4j.conf:
          dbms.directories.import=/path/to/import/directory
       3) Update connection details in the App.java file if necessary.

# 3. Build the Project
        mvn clean install

# 4. Run the Application
       java -jar target/web-crawler.jar
       
       1) Input the starting URL and maximum crawl depth when prompted.
       2) Analyze and visualize results in Neo4j.

# Key Neo4j Queries

# Page Storage
# Nodes for Pages:
     MATCH (p:Page)
     RETURN p.url, p.title;
# Links Between Pages:
     MATCH (p1:Page)-[:LINKS_TO]->(p2:Page)
     RETURN p1.url, p2.url;
# Graph Analytics

# Most Outgoing Links:
     MATCH (p:Page)-[:LINKS_TO]->()
     RETURN p.url, COUNT(*) AS OutgoingLinks
     ORDER BY OutgoingLinks DESC;
# PageRank Calculation:
    CALL gds.pageRank.write({
      nodeProjection: 'Page',
      relationshipProjection: { LINKS_TO: { type: 'LINKS_TO' } },
      writeProperty: 'pageRank'
    });

# Dead Ends:
    MATCH (p:Page)
    WHERE NOT (p)-[:LINKS_TO]->()
    RETURN p.url;
# Visualization

   # Display the Graph:
     MATCH (p:Page)-[r:LINKS_TO]->(q:Page)
     RETURN p, r, q;

# Results

    1) Proportional visualization of outgoing links.

    2) Analysis of crawling performance, including pages crawled per second.
 
    3) Insights into the connectivity and structure of the crawled web.


   
