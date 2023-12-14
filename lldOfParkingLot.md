## LLD of Parking lot

Requirement Gathering / Clarification:

How many Entrances?
    - 1 Entrance
    - 1 Exit
    - Different type of parking spots:
        - 2 Wheeler
        - 4 Wheeler
    - Payment: Hourly / Minute based charge => Mix
    - Floors? => No

Objects:

- Vehicle
    1 Vehicle No.
    2 Vehicle Type (Enum)
- Ticket
    1. Entry Time
    2. Parking Spot
- Entrance Gate
    1. Find Parking Space (Nearest Parking to the entrance)
    2. Update Parking space
    3. Generate Ticket
- Parking Spot
      1. id
    2. isEmpty
    3. Vehicle
    4. Price
    5. Type
- Exit Gate
    1. Cost Calculation
    2. Payment
    3. Update Parking Spot

2 Approaches possible -
    1 Top Down
    2 Bottom Up <-

Design Patterns Used: Strategy, Factory, Dependency Inversion    
