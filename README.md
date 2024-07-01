# Praktikum 6

## ER-D Praktikum 6
![](Foto/erd.png)

## Input Data Studi Kasus Karyawan
![](Foto/inputdata.png)

### *Tabel Perusahan*
```sql
CREATE TABLE Perusahaan(
id_p VARCHAR(10) PRIMARY KEY NOT NULL,
nama VARCHAR(45),
alamat VARCHAR(45) 
);

INSERT INTO Perusahaan (id_p, nama, alamat) VALUES
('P01', 'Kantor Pusat', NULL),
('P02', 'Cabang Bekasi', NULL);
```

### *Output Tabel Perusahaan :*
![](Foto/perusahaan.png)

### *Tabel Departemen*
```sql
CREATE TABLE Departemen(
id_dept VARCHAR(10) PRIMARY KEY NOT NULL,
nama VARCHAR(45),
id_p VARCHAR(10),
manajer_nik VARCHAR(10),
FOREIGN KEY (id_p) REFERENCES Perusahaan(id_p)
);

INSERT INTO Departemen (id_dept, nama, id_p, manajer_nik) VALUES
('D01', 'Produksi', 'P02', 'N01'),
('D02', 'Marketing', 'P01', 'N03'),
('D03', 'RnD', 'P02', NULL),
('D04', 'Logistik', 'P02', NULL);
```
### *Output Tabel Departemen :*
![](Foto/departemen.png)

### *Tabel Karyawan*
```sql
CREATE TABLE Karyawan(
nik VARCHAR(10) PRIMARY KEY NOT NULL,
nama VARCHAR(45),
id_dept VARCHAR(10),
sup_nik VARCHAR(10),
FOREIGN KEY (id_dept) REFERENCES Departemen(id_dept)
);

INSERT INTO Karyawan (nik, nama, id_dept, sup_nik) VALUES
('N01', 'Ari', 'D01', NULL),
('N02', 'Dina', 'D01', NULL),
('N03', 'Rika', 'D03', NULL),
('N04', 'Ratih', 'D01', 'N01'),
('N05', 'Riko', 'D01', 'N01'),
('N06', 'Dani', 'D02', NULL),
('N07', 'Anis', 'D02', 'N06'),
('N08', 'Dika', 'D02', 'N06');
```
### *Output Tabel Karyawan :*
![](Foto/karyawan.png)

### *Tabel Project*
```sql
CREATE TABLE Project(
id_proj VARCHAR(10) PRIMARY KEY NOT NULL,
nama VARCHAR(45),
tgl_mulai DATETIME,
tgl_selesai DATETIME,
status TINYINT(1)
);

INSERT INTO Project (id_proj, nama, tgl_mulai, tgl_selesai, status) VALUES
('PJ01', 'A', '2019-01-10', '2019-03-10', '1'),
('PJ02', 'B', '2019-02-15', '2019-04-10', '1'),
('PJ03', 'C', '2019-03-21', '2019-05-10', '1');
```

### *Output Tabel Project :*
![](Foto/project.png)

### *Tabel Project Detail*
```sql
CREATE TABLE Project_detail(
id_proj VARCHAR(10) NOT NULL,
nik VARCHAR(10) NOT NULL
);

INSERT INTO Project_detail (id_proj, nik) VALUES
('PJ01', 'N01'),
('PJ01', 'N02'),
('PJ01', 'N03'),
('PJ01', 'N04'),
('PJ01', 'N05'),
('PJ01', 'N07'),
('PJ01', 'N08'),
('PJ02', 'N01'),
('PJ02', 'N03'),
('PJ02', 'N05'),
('PJ03', 'N03'),
('PJ03', 'N07'),
('PJ03', 'N08');
```

### *Output Project Detail :*
![](Foto/projectdetail.png)

## Latihan Praktikum 6

### 1. Departemen apa saja yang terlibat dalam tiap-tiap Project
**Script :**

```sql
SELECT Project.nama AS Project, GROUP_CONCAT(Departemen.nama) AS Departemen
FROM Project
INNER JOIN Project_detail ON Project.id_proj = Project_detail.id_proj
INNER JOIN Karyawan ON Project_detail.nik = Karyawan.nik
INNER JOIN Departemen ON Karyawan.id_dept = Departemen.id_dept
GROUP BY Project.id_proj;
```

**Output :**

![](Foto/1.png)

### 2. Jumlah Karyawan tiap Departemen yang bekerja pada tiap-tiap project
**Script :**

```sql
SELECT Project.nama AS Project, Departemen.nama AS Departemen, COUNT(*) AS 'Jumlah Karyawan'
FROM Project
INNER JOIN Project_detail ON Project.id_proj = Project_detail.id_proj
INNER JOIN Karyawan ON Project_detail.nik = Karyawan.nik
INNER JOIN Departemen ON Karyawan.id_dept = Departemen.id_dept
GROUP BY Project.id_proj, Departemen.id_dept;
```

**Output :**

![](Foto/2.png)

### 3. Ada berapa Project yang sedang dikerjakan oleh Departemen *RnD*? *(ket: project berjalan adalah yang statusnya 1)*
**Script :**

```sql
SELECT COUNT(*) AS 'Jumlah Project'
FROM Project
INNER JOIN Project_detail ON Project.id_proj = Project_detail.id_proj
INNER JOIN Karyawan ON Project_detail.nik = Karyawan.nik
INNER JOIN Departemen ON Karyawan.id_dept = Departemen.id_dept
WHERE Departemen.nama = 'RnD' AND Project.status = 1;
```

**Output :**

![](Foto/3.png)

### 4. Berapa banyak Project yang sedang dikerjakan oleh Ari?
**Script :**

```sql
SELECT COUNT(*) AS 'Jumlah Project'
FROM Project_detail
INNER JOIN Karyawan ON Project_detail.nik = Karyawan.nik
WHERE Karyawan.nama = 'Ari' AND Project_detail.id_proj IN (SELECT id_proj FROM Project WHERE status = 1);
```

**Output :**

![](Foto/4.png)

### 5. Siapa saja yang mengerjakan Project B?
**Script :**

```sql
SELECT Karyawan.nama
FROM Project_detail
INNER JOIN Karyawan ON Project_detail.nik = Karyawan.nik
WHERE Project_detail.id_proj IN (SELECT id_proj FROM Project WHERE nama = 'B');
```

**Output :**

![](Foto/5.png)

