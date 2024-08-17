## Future considerations:

### Long term success:

- Adoption by at least two major auction houses. Having more than one auction house will help development of an open standard.

- Adoption by at least 10 major artist. Art is at the heart of the project. Buy-in from major artists is sine qua non.

- Adoption by at least 5 major contemporary art museums around the world. Museums have a vested stake in making sure art is both availalbe to the public and authentic. Interest from different museums will incentivize artists to join.


### Storing the authentication/ownership

Title of an artwork could be stored in some sort of database or blockchain so that if I own print 3/10 and someone makes another 3/10, their copy will appear legitmate, but they won't be able to prove they own it. Additionally, the artist (or posthumously, their foundation) could use that same data store to store information about the artwork, like the number of editions. That way, if--using the above example, I have print 3/10 from the _first_ edition and the only other authentic edition is a second edition of 5 prints, trying to print a third edition will be an obvious forgery.


Additionally if a collector sells or gifts their artwork a new record in a database or new block in a blockchain could store that information. The DB or blockchain might hold something like:

| id | collector |                  artwork                 | previous collector |
| ---| --------- | ---------------------------------------- | ------------------ |
| 1  | John      | "Funk Lessons" 1st edition 3/5           |        NULL        |
| 2  | Jane      | "My Calling (Card) #1" 2st edition 28/50 |        John        |