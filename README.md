# SQL_NOVI_opdracht
```
DROP TABLE IF EXISTS remote_controller;
DROP TABLE IF EXISTS ci_module;
DROP TABLE IF EXISTS television_wallbracket;
DROP TABLE IF EXISTS wallbracket;
DROP TABLE IF EXISTS television;

CREATE TABLE television (
	id SERIAL PRIMARY KEY,
	name text,
	brand text,
	price double precision,
	current_stock integer,
	sold integer,
	type text NOT NULL UNIQUE,
	available integer DEFAULT 0,
	refresh_rate double precision,
	screen_type text
);

CREATE TABLE remote_controller (
	id SERIAL PRIMARY KEY,
	name text,
	brand text,
	price double precision,
	current_stock integer,
	sold integer,
	compatible_with text,
	battery_type text,
	television_id integer,
	CONSTRAINT fk_television FOREIGN KEY (television_id) REFERENCES television(id)
);

CREATE TABLE ci_module (
	id SERIAL PRIMARY KEY,
	name text,
	brand text,
	price double precision,
	current_stock integer,
	sold integer,
	television_id integer,
	CONSTRAINT fk_television FOREIGN KEY (television_id) REFERENCES television(id)
);

CREATE TABLE wallbracket (
	id SERIAL PRIMARY KEY,
	name text,
	brand text,
	price double precision,
	current_stock integer,
	sold integer,
	adjustable boolean,
	television_id integer,
	CONSTRAINT fk_television FOREIGN KEY (television_id) REFERENCES television(id)
);

CREATE TABLE television_wallbracket (
	television_id integer,
	wallbracket_id integer,
	CONSTRAINT fk_television FOREIGN KEY (television_id) REFERENCES television(id),
	CONSTRAINT fk_wallbracket FOREIGN KEY (wallbracket_id) REFERENCES wallbracket(id)
);

INSERT INTO television(name, brand, price, current_stock, sold, type, available, refresh_rate, screen_type)
	VALUES ('Oled', 'Samsung', 699, 5, 1, '5lq-sw', 5, 66.5, 'led'),
		('LED', 'Philips', 399, 9, 3, 'dsf-sw', 5, 22.5, 'led');
	
INSERT INTO remote_controller(name, brand, price, current_stock, sold, compatible_with, battery_type, television_id)
	VALUES ('test1', 'Yamaha', 50, 5, 1, 'Samsung', 'AAA', (SELECT id FROM television WHERE type = '5lq-sw'));

INSERT INTO ci_module(name, brand, price, current_stock, sold) 
	VALUES ('name', 'brand', 85, 3, 2);

INSERT INTO wallbracket(name, brand, price, current_stock, sold, adjustable)
	VALUES ('name bracket', 'brand bracket', 35, 7, 2, true);

SELECT * FROM  television
	JOIN remote_controller ON television.id = remote_controller.television_id
	JOIN ci_module ON television.id = ci_module.television_id
	JOIN wallbracket ON television.id = wallbracket.television_id;
```
