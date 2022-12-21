-- create a table

CREATE TABLE Pilots (
  pilot_id INTEGER PRIMARY KEY,
  name TEXT,
  age INTEGER,
  rank TEXT,
  education_level TEXT
);

CREATE TABLE Plane (
  plane_id INTEGER PRIMARY KEY,
  capacity INTEGER,
  cargo_flg BIT 
);

CREATE TABLE Reis (
  flight_id INTEGER,
  flight_dt date,
  first_pilot_id INTEGER,
  second_pilot_id INTEGER,
  plane_id INTEGER,
  destination TEXT,
  quantity INTEGER
  primary key (flight_id, flight_dt)
  FOREIGN KEY (first_pilot_id)  REFERENCES Pilots (pilot_id),
  FOREIGN KEY (plane_id)  REFERENCES Plane (plane_id),
  FOREIGN KEY (second_pilot_id)  REFERENCES Pilots (pilot_id)
);
-- insert some values
INSERT INTO Pilots VALUES (1, 'Fisrt fly 3 times', 46,'rank1','school');
INSERT INTO Pilots VALUES (2, 'Second fly 2 times', 47,'rank1','school');
INSERT INTO Pilots VALUES (3, 'Dont Fly sheremetevo', 23,'rank1','school');
INSERT INTO Pilots VALUES (4, 'Dont Fly this year', 23,'rank1','school');
INSERT INTO Pilots VALUES (5, 'Fly only first',  23,'rank1','school');
INSERT INTO Pilots VALUES (6, 'Fly lke 1 3 times',  23,'rank1','school');

INSERT into Plane values (1,10,0);
INSERT into Plane values (2,10,0);
INSERT into Plane values (3,30,1);
INSERT into Plane values (4,40,1);
INSERT into Plane values (5,50,1);

INSERT into Reis values(1,'2022-08-02',5,1,3,'Шереметьево',14)
INSERT into Reis values(2,'2022-08-03',5,1,4,'Шереметьево',14)
INSERT into Reis values(3,'2022-08-04',5,1,5,'Шереметьево',14)
INSERT into Reis values(4,'2022-08-03',5,3,3,'Абакан',14)
INSERT into Reis values(5,'2022-08-04',5,2,4,'Шереметьево',14)
INSERT into Reis values(6,'2021-08-05',5,4,5,'Шереметьево',14)
INSERT into Reis values(7,'2022-08-02',5,6,3,'Шереметьево',14)
INSERT into Reis values(8,'2022-08-02',5,6,4,'Шереметьево',14)
INSERT into Reis values(9,'2022-08-03',5,6,5,'Шереметьево',14)
INSERT into Reis values(10,'2022-08-05',5,2,3,'Шереметьево',14)
INSERT into Reis values(11,'2022-08-05',1,2,1,'Шереметьево',14)
INSERT into Reis values(12,'2022-08-05',1,2,2,'Шереметьево',14)
INSERT into Reis values(13,'2022-08-05',2,2,1,'Шереметьево',14)
INSERT into Reis values(14,'2023-08-05',3,2,2,'Шереметьево',14)



SELECT pilot_id,name,age,rank,education_level FROM Pilots as t1 join (SELECT second_pilot_id, COUNT(second_pilot_id) as schetchik FROM reis WHERE destination like 'Шереметьево' and flight_dt BETWEEN '2022-08-01' AND '2022-08-31' group by second_pilot_id) t2 on t1.pilot_id=t2.second_pilot_id where t2.schetchik=3



Select pilot_id from Pilots as t1 
join(Select first_pilot_id,second_pilot_id from Reis as t1 join Plane as t2 on t1.plane_id = t2.plane_id where t2.cargo_flg=1 and t2.capacity>30) t2
on t1.pilot_id=t2.first_pilot_id or t1.pilot_id=t2.second_pilot_id where t1.age>45 group by t1.pilot_id



Select TOP(10) first_pilot_id from
(Select t1.first_pilot_id,Count(t1.flight_id)flys from Reis as t1 join Plane as t2 on t1.plane_id=t2.plane_id where t2.cargo_flg=0 and t1.flight_dt between '2022-01-01' AND '2022-12-31' group by t1.first_pilot_id) t1
order by flys DESC

Drop Table Plane;
Drop Table Reis;
Drop table Pilots;

