CREATE TRIGGER nama_trigger trigger_time trigger_even -- 6 event
ON table_name
FOR EACH ROW

BEGIN 
	-- trigger body (sql statement)
END;

-- select * from tb_master_employee;
-- create trigger 



-- Create table log employee dulu
CREATE TABLE tb_master_employee_log(
	tb_log_id int PRIMARY KEY AUTO_INCREMENT,
	tb_log_emp_id int,ALTER TABLE view_procedure.tb_master_employee ADD `tbme_health limit` int NULL;
	tb_log_old_nama_depan varchar(100),
	tb_log_new_nama_depan varchar(100),
	tb_log_change_date datetime
	
); 

SELECT * FROM tb_master_employee_log;

CREATE OR REPLACE TRIGGER tr_empl_log BEFORE UPDATE
	ON tb_master_employee 
	FOR EACH ROW 
BEGIN 
	INSERT INTO tb_master_employee_log
	SET tb_log_emp_id = OLD.tbme_id,
		tb_log_old_nama_depan = OLD.tbme_firstname,
		tb_log_new_nama_depan = NEW.tbme_firstname,
		tb_log_change_date = now();
END;

SHOW TRIGGERS;

-- Show table
SELECT * FROM tb_master_employee WHERE tbme_id = 3427; -- tabel sumber
SELECT * FROM tb_master_employee_log WHERE tb_log_emp_id = 3427; -- tabel log jika trigger jalan

UPDATE tb_master_employee SET tbme_firstname = 'Uria S' WHERE tbme_id = 3427; -- ex: update




-- Menampilkan trigger
SHOW TRIGGERS;

-- SHOW TRIGGERS IN nama_database;
   SHOW TRIGGERS IN pemrogramanbasisdata;
  
-- SHOW TRIGGERS IN nama_database LIKE '%nama_tabel%';   
   SHOW TRIGGERS IN view_procedure LIKE '%%'; -- bisa untuk cari issue trigger jalan yg tidak diharapkan. 

-- show triggers berdasarkan event INSERT|UPDATE|DELETE
   SHOW TRIGGERS IN view_procedure WHERE `event` = 'UPDATE';
  
USE view_procedure;
-- contoh ketika lock  dengan READ atau WRITE
LOCK TABLES tb_master_employee WRITE;

LOCK TABLES tb_master_employee READ;

UNLOCK TABLES; 

SELECT * FROM tb_master_employee; 



-- Contoh menggunakan mekanisme lock ketika hapus trigger:
-- SHOW TRIGGERS;
LOCK TABLES tb_master_employee WRITE;

DROP TRIGGER tr_empl_log; -- hapus trigger

CREATE TRIGGER tr_empl_log BEFORE UPDATE
	ON tb_master_employee 
	FOR EACH ROW 
BEGIN 
	INSERT INTO tb_master_employee_log
	SET tb_log_emp_id = OLD.tbme_id,
		tb_log_old_nama_depan = OLD.tbme_firstname,
		tb_log_new_nama_depan = NEW.tbme_firstname,
		tb_log_change_date = now();
END;

UNLOCK TABLES; -- RELEASE LOCK

-- Cek lamanya nunggu ketika locking table aktif:
SHOW VARIABLES LIKE 'lock_wait_timeout';

SET session lock_wait_timeout = 10;
SHOW VARIABLES LIKE 'lock_wait_timeout';


-- BEFORE & AFTER INSERT TRIGGER
-- BEFORE INSERT
CREATE OR REPLACE TRIGGER tr_empl_before_insert BEFORE INSERT
	ON tb_master_employee 
	FOR EACH ROW 
BEGIN 
	CASE WHEN NEW.tbme_current_employee_rating < 3 THEN
		SET NEW.tbme_current_employee_rating = 3;
	END CASE;
END;

SELECT * FROM tb_master_employee; 

INSERT INTO tb_master_employee
(tbme_firstname, tbme_lastname, tbme_star_date, tbme_exit_date, tbme_title, tbme_supervisor, tbme_ad_email, tbme_business_unit, tbme_employee_status, tbme_employee_type, tbme_pay_zone, tbme_employee_classification_type, tbme_termination_type, tbme_terminatio_description, tbme_department_type, tbme_division, tbme_dob, tbme_state, tbme_job_function_description, tbme_gender_code, tbme_location_code, tbme_race_desc, tbme_marital_desc, tbme_performance_score, tbme_current_employee_rating)
VALUES('deni 23', 'Bridges', '20-Sep-19', '', 'Production Technician I', 'Peter Oneill', 'uriah.bridges@bilearner.com', 'CCDR', 'Active', 'Contract', 'Zone C', 'Temporary', 'Unk', '', 'Production       ', 'Finance & Accounting', '07-10-1969', 'MA', 'Accounting', 'Female', 34904, 'White', 'Menikah', 'Fully Meets', 1);

SELECT * FROM tb_master_employee where tbme_firstname ='deni 23';

-- AFTER INSERT 

-- Tambahkan field limit healt untuk mengetes AFTER INSERT
ALTER TABLE tb_master_employee ADD tbme_health_limit int;

UPDATE tb_master_employee SET tbme_health_limit = 10000000; -- isi semua data limit ke 10 jt

-- TABEL UNTUK MENYIMPAN TRIGGER AFTER INSERT
CREATE TABLE tb_master_employee_health_redeem(
	tbme_id,
	tbme_amount_redeem int,
	tbme_created_date timestamp
);

-- TRIGGER AFTER INSERT Save amount health
CREATE TRIGGER tr_empl_after_insert AFTER INSERT
	ON tb_master_employee_health_redeem 
	FOR EACH ROW 
BEGIN 
	UPDATE tb_master_employee 
		SET tbme_health_limit=(tbme_health_limit-NEW.tbme_amount_redeem)
	WHERE tbme_id=NEW.tbme_id;
END;

-- Masukan data untuk test BEFORE INSERT
INSERT
	INTO tb_master_employee_health_redeem (tbme_id,tbme_amount_redeem,tbme_created_date)
    VALUES(3000,1000000,now());
-- cek data after BEFORE INSERT
SELECT * FROM tb_master_employee_health_redeem;
SELECT * FROM tb_master_employee WHERE tbme_id =3000;


-- BEFORE & AFTER UPDATE TRIGGER

-- BEFORE UPDATE
CREATE OR REPLACE TRIGGER tr_empl_before_update_log BEFORE UPDATE
	ON tb_master_employee 
	FOR EACH ROW 
BEGIN 	
	CASE WHEN NEW.tbme_current_employee_rating < 3 THEN
		SET NEW.tbme_current_employee_rating = 3;
	END CASE;
END;


-- AFTER UPDATE 
CREATE OR REPLACE TRIGGER tr_empl_after_update_log AFTER UPDATE
	ON tb_master_employee 
	FOR EACH ROW 
BEGIN 	
	INSERT INTO tb_master_employee_log
	SET tb_log_emp_id = OLD.tbme_id,
		tb_log_old_nama_depan = OLD.tbme_firstname,
		tb_log_new_nama_depan = NEW.tbme_firstname,
		tb_log_change_date = now();
END;



-- Jalankan UPDATE statement untuk mencoba TRIGER UPDATE
UPDATE tb_master_employee SET tbme_firstname = 'Susae 55', tbme_current_employee_rating = 1 WHERE tbme_id = 1001; -- ex: update

SELECT * FROM tb_master_employee_log WHERE tb_log_emp_id =1001;
SELECT * FROM tb_master_employee where tbme_id = 1001 order by tbme_id desc; 



-- BEFORE AFTER DELETE TRIGGER
-- Tabel untuk test DELETE TRIGGER
CREATE TABLE tb_master_employee_history(
    tbme_id int primary key auto_increment,
	tbme_emp_id int,
	tbme_amount_redeem int,
	tbme_created_date timestamp
);

-- BEFORE DELETE 
CREATE OR REPLACE TRIGGER tr_empl_before_delete BEFORE DELETE
	ON tb_master_employee 
	FOR EACH ROW 
BEGIN 	
	INSERT INTO tb_master_employee_history
	SET tbme_emp_id = OLD.tbme_id,
		tbme_sisa_amount_redeem  = OLD.tbme_health_limit,
		tbme_created_date  = now();
END;

-- DELETE statement untuk test BEFORE DELETE
DELETE FROM tb_master_employee WHERE tbme_id =3001;

SELECT * FROM tb_master_employee WHERE tbme_id = 3001;
SELECT * FROM tb_master_employee_history; 




