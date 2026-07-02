Clinic Management Portal
A secure, role-based, relational medical clinic management platform built with PHP (PDO) and MySQL. This system streamlines patient onboarding, diagnostic tracking, prescription writing, pharmaceutical fulfillment, and patient self-service entry.

🚀 Features
👤 Role-Based Portals & Permissions
Admin: Full master directory oversight, system diagnostics monitoring, and configuration visibility.

Doctor: Full clinical management capabilities. Can register new patients, enter diagnostic evaluations, and issue real-time prescriptions.

Pharmacist: Read-only access to patient health trackers and a clean, visual status system indicating which orders are ready to be dispensed.

Patient: A private, read-only interface (my_medical_file.php) where patients can manage their contact information and view their entire consultation history, tracked symptoms, and medication details.

🛡️ Core Security Infrastructure
PDO Prepared Statements: Full defense against SQL injection attacks across all endpoints.

Cross-Site Scripting (XSS) Mitigation: Complete structural context protection via output sanitization utilizing htmlspecialchars().

Database Transaction Protection: Cross-table insertion safety via $pdo->beginTransaction(), $pdo->commit(), and $pdo->rollBack() routines.

📊 System Architecture & Schema
The portal operates on a single database containing three closely linked relational tables using Patient_ID and Consultation_ID as relational mapping anchors.

Patients: Houses primary demographic indices, user portal profile identities, and authentication profiles.

Consultations: Tracks clinical history entries, including recorded symptoms and medical diagnoses.

Prescriptions: Links specific medication instruction scripts directly to their parent consultation encounter.

🛠️ Installation & Environment Setup
Prerequisites
XAMPP / WAMP / MAMP (PHP 7.4 or higher, MySQL / MariaDB Server)

Web Browser (Chrome, Firefox, Edge, etc.)

Step 1: Clone or Copy Project Files
Move all project files (including patients.php, my_medical_file.php, and db.php) into your local server environment's root directory:

Bash
# Example for XAMPP
C:\xampp\htdocs\clinic_management\
Step 2: Database Setup
Start Apache and MySQL via your XAMPP Control Panel.

Open your web browser and navigate to http://localhost/phpmyadmin/.

Create a new database named clinic_db.

Import your SQL schema or execute the following query to build the required architecture:

SQL
CREATE TABLE IF NOT EXISTS Patients (
    Patient_ID INT AUTO_INCREMENT PRIMARY KEY,
    Full_Name VARCHAR(100) NOT NULL,
    Username VARCHAR(50) NOT NULL UNIQUE,
    Password VARCHAR(255) NOT NULL,
    Phone VARCHAR(20),
    Address TEXT,
    DOB DATE,
    Gender VARCHAR(20),
    Blood_Group VARCHAR(10)
);

CREATE TABLE IF NOT EXISTS Consultations (
    Consultation_ID INT AUTO_INCREMENT PRIMARY KEY,
    Patient_ID INT NOT NULL,
    Diagnosis TEXT NOT NULL,
    Symptoms TEXT,
    Consultation_Date DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (Patient_ID) REFERENCES Patients(Patient_ID) ON DELETE CASCADE
);

CREATE TABLE IF NOT EXISTS Prescriptions (
    Prescription_ID INT AUTO_INCREMENT PRIMARY KEY,
    Consultation_ID INT NOT NULL,
    Medication_Details TEXT NOT NULL,
    FOREIGN KEY (Consultation_ID) REFERENCES Consultations(Consultation_ID) ON DELETE CASCADE
);
Step 3: Configure the Database Connection
Open db.php in your code editor and verify that your credentials match your environment settings:

PHP
<?php
try {
    $pdo = new PDO('mysql:host=localhost;dbname=clinic_db;charset=utf8', 'root', '');
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    $pdo->setAttribute(PDO::ATTR_DEFAULT_FETCH_MODE, PDO::FETCH_ASSOC);
} catch (PDOException $e) {
    die("Database Connection Failed: " . $e->getMessage());
}
Step 4: Accessing the Portal
Navigate to the root location inside your browser:

HTML
http://localhost/clinic_management/patients.php
📄 Key File Architecture Reference
db.php: Core data connection hub that establishes data streaming via explicit PDO configurations.

patients.php: The foundational control interface for Admins, Doctors, and Pharmacists. Controls validation, inserts profiles, manages lookup entries, and binds medical details.

my_medical_file.php: The patient endpoint. Provides contact info management and details historical clinical notes via standard table relations.
