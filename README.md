# oop-crud
Employee Management System


config.php
---------------------------------------------
<?php

	define("DB_HOST", "localhost");
	define("DB_USER", "root");
	define("DB_PASS", "");
	define("DB_NAME", "themehippo");

Database.php
--------------------------------------------
<?php
Class Database {

	public $host 	= DB_HOST;
	public $user 	= DB_USER;
	public $pass 	= DB_PASS;
	public $dbname 	= DB_NAME;

	public $link;
	public $error;

	public function __construct() { 
		$this->connectionDB();
	}
	private function connectionDB() {
		$this->link = new mysqli($this->host, $this->user, $this->pass, $this->dbname);

		if (!$this->link) {
			$this->error = "Connection Fail !".$this->link->connect_error;
			return false;
		}
	}

	//Database Select 

	public function select($query) {
		$result = $this->link->query($query) or die ($this->link->error.__LINE__);
		if ($result->num_rows > 0) {
			return $result;
		}else {
			return false;
		}

	}

	//Database Insert
	public function insert($query) {
		$insert_row = $this->link->query($query) or die ($this->link->error.__LINE__);
		if ($insert_row) {
			header("Location: index.php?msg=".urlencode("Data Insert Successfully . "));
			exit;
		}else {
			die("Error: (".$this->link->errno.") ".$this->link->error);
		}
	}

    //Update Data
	public function update($query) {
		$update_row = $this->link->query($query) or die ($this->link->error.__LINE__);
		if ($update_row) {
			header("Location: index.php?msg=".urlencode("Data Update Successfully . "));
			exit;
		}else {
			die("Error: (".$this->link->errno.") ".$this->link->error);
		}
	}

    //Delete Data
	public function delete($query) {
		$delete_row = $this->link->query($query) or die ($this->link->error.__LINE__);
		if ($delete_row) {
			header("Location: index.php?msg=".urlencode("Data Delete Successfully . "));
			exit;
		}else {
			die("Error: (".$this->link->errno.") ".$this->link->error);
		}
	}

}

index.php
---------------------------------------------
<?php 
	include "config.php";
	include "Database.php";

	$db = new Database();
	$query = "SELECT * FROM employee_salary";
	$read = $db->select($query);

?>

<!DOCTYPE html>
<html lang="en-US">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initail-scale=1" />
	<title>ThemeHippo | Employee Management System</title> 

	<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/css/bootstrap.min.css" rel="stylesheet" crossorigin="anonymous">
    <link rel="stylesheet" href="assets/css/custom.css">
    <link rel="shortcut icon" href="assets/images/34fb4-1ad51.ico" type="image/x-icon">

</head>
<body>

    <div class="main-site">
		<div class="container">
			<div class="sec-heading">
				<h2>ThemeHippo Employee Management System</h2>
                <hr>
				<p> Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
				tempor incididunt ut labore et dolore magna aliqua.</p>
			</div>

            <div class="table-form">
                <div class="row">
                    <div class="col-md-12">
                        <?php
                            if (isset($_GET['msg'])) {
                                echo "<span style='color: green'>".$_GET['msg']."</span>";
                            }
                        ?>
                        <div class="date-time">
                            <p>April - 19 - 2021</p> 
                        </div>
                        <table class="table table-striped">
                            <thead>
                            <thead>
                                <tr>
                                    <th scope="col">FULL NAME</th>
                                    <th scope="col">ID</th>
                                    <th scope="col">SALARY</th>
                                    <th scope="col">Acltion</th>
                                </tr>
                            </thead>
                        <tbody>
                        <?php if ($read) : ?>
					  		<?php while ($row = $read->fetch_assoc()) : ?>
                                <tr>
                                    <td><?php echo $row['fname']; ?></td>
                                    <td><?php echo $row['idno']; ?></td>
                                    <td><?php echo $row['salary']; ?></td>
                                    <td><a href="update.php?id=<?php echo urlencode($row['id']); ?>">Edit</a></td>
                                </tr>
                            <?php endwhile; ?>
					  	<?php endif; ?>

                        </tbody>
                        </table>
                        <a href="form.php">Add Employee</a>
                    </div>
                </div>
            </div>

		</div>
	</div>

</body>


create.php/ form.php
------------------------------------------
<?php 
	include "config.php";
	include "Database.php";

	$db = new Database();
	if (isset($_POST['submit'])) {
		$fname = mysqli_real_escape_string($db->link, $_POST['fname']);
		$idno = mysqli_real_escape_string($db->link, $_POST['idno']);
		$salary = mysqli_real_escape_string($db->link, $_POST['salary']);

		if ($fname == '' || $idno == '' || $salary == '') {
			$error = "Field Must Not Be Empty";
		}else {
			$query = "INSERT INTO employee_salary(fname, idno, salary) Values('$fname', '$idno', '$salary')";
			$create = $db->insert($query);
		}

	}
?>


<!DOCTYPE html>
<html lang="en-US">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initail-scale=1" />
	<title>ThemeHippo | Employee Management System</title> 

	<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/css/bootstrap.min.css" rel="stylesheet" crossorigin="anonymous">
    <link rel="stylesheet" href="assets/css/custom.css">
    <link rel="shortcut icon" href="assets/images/34fb4-1ad51.ico" type="image/x-icon">

</head>
<body>

    <div class="main-site">
		<div class="container">
			<div class="sec-heading">
				<h2>ThemeHippo Add Employee</h2>
                <hr>
				<p> Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
				tempor incididunt ut labore et dolore magna aliqua.</p>
			</div>

            <div class="insert-form">
                <div class="validation-error">
                    <?php
						if (isset($error)) {
							echo "<span style='color: red'>".$error."</span>";
						}
					?>
                </div>

                <form method="post" action="form.php" class="in-form">
                    <p>Full Name*</p>
                    <input type="text" name="fname" class="form-control" placeholder="Md. Emran Ahmed">
                    <p>Id No*</p>
                    <input type="text" name="idno" class="form-control" placeholder="#SD-101">
                    <p>Salary*</p>
                    <input type="text" name="salary" class="form-control" placeholder="000.01"><br>

                    <input type="submit" name="submit" value="Submit" class="btn btn-info">
                    <input type="reset" value="Cancel" class="btn btn-danger">
                </form>
                <a href="index.php">Go Back</a>
            </div>
            
        </div>
    </div>
</body>
</html>


Update.php
----------------------------------
<?php 
	include "config.php";
	include "Database.php";

	$db = new Database();
 
	$id = $_GET['id'];
	$query = "SELECT * FROM employee_salary WHERE id=$id";
	$getData = $db->select($query)->fetch_assoc();

	if (isset($_POST['submit'])) {
		$fname = mysqli_real_escape_string($db->link, $_POST['fname']);
		$idno = mysqli_real_escape_string($db->link, $_POST['idno']);
		$salary = mysqli_real_escape_string($db->link, $_POST['salary']);

		if ($fname == '' || $idno == '' || $salary == '') {
			$error = "Field Must Not Be Empty";
		}else {
			$query = "UPDATE employee_salary 
				SET 
				fname 	= '$fname',
				idno 	= '$idno',
				salary 	= '$salary'
				WHERE id=$id";
			$update = $db->update($query);
		}

	}
?> 
<?php
    if(isset($_POST['delete'])) {
        $query = "DELETE FROM employee_salary WHERE id=$id";
        $deleteData = $db->delete($query);
    }
?>

<!DOCTYPE html>
<html lang="en-US">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initail-scale=1" />
	<title>ThemeHippo | Employee Management System</title> 

	<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/css/bootstrap.min.css" rel="stylesheet" crossorigin="anonymous">
    <link rel="stylesheet" href="assets/css/custom.css">
    <link rel="shortcut icon" href="assets/images/34fb4-1ad51.ico" type="image/x-icon">

</head>
<body>

    <div class="main-site">
		<div class="container">
			<div class="sec-heading">
				<h2>ThemeHippo Update Employee</h2>
                <hr>
				<p> Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
				tempor incididunt ut labore et dolore magna aliqua.</p>
			</div>

            <div class="insert-form">
                <div class="validation-error">
                    <?php
						if (isset($error)) {
							echo "<span style='color: red'>".$error."</span>";
						}
					?>
                </div>

                <form method="post" action="update.php?id=<?php echo $id; ?>" class="in-form">
                    <p>Full Name*</p>
                    <input type="text" name="fname" class="form-control" value="<?php echo $getData['fname']; ?>">
                    <p>Id No*</p>
                    <input type="text" name="idno" class="form-control" value="<?php echo $getData['idno']; ?>">
                    <p>Salary*</p>
                    <input type="text" name="salary" class="form-control" value="<?php echo $getData['salary']; ?>"><br>

                    <input type="submit" name="submit" value="Update" class="btn btn-info">
                    <input type="reset" value="Cancel" class="btn btn-warning">
                    <input type="submit" name="delete" value="Delete" class="btn btn-danger">

                </form>
                <a href="index.php">Go Back</a>
            </div>
            
        </div>
    </div>
</body>
</html>


employee_data.sql
----------------------------------
-- phpMyAdmin SQL Dump
-- version 5.0.4
-- https://www.phpmyadmin.net/
--
-- Host: 127.0.0.1
-- Generation Time: Apr 18, 2021 at 11:51 PM
-- Server version: 10.4.17-MariaDB
-- PHP Version: 8.0.0

SET SQL_MODE = "NO_AUTO_VALUE_ON_ZERO";
START TRANSACTION;
SET time_zone = "+00:00";


/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8mb4 */;

--
-- Database: `themehippo`
--
--
-- Table structure for table `employee_salary`
--

CREATE TABLE `employee_salary` (
  `id` int(11) NOT NULL,
  `fname` varchar(255) NOT NULL,
  `idno` varchar(255) NOT NULL,
  `salary` varchar(255) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

--
-- Dumping data for table `employee_salary`
--

INSERT INTO `employee_salary` (`id`, `fname`, `idno`, `salary`) VALUES
(3, 'Md. Mostakim Islam', 'SD-105', '45,000'),
(4, 'Md.  Al Mamun', 'TH-102', '55,000'),
(5, 'Md. Rokonuzzaman ', 'SD-101', '80,000'),
(6, 'Md. Robiul Islam', 'SD-109', '70,000');

--
-- Indexes for dumped tables
--

--
-- Indexes for table `employee_salary`
--
ALTER TABLE `employee_salary`
  ADD PRIMARY KEY (`id`);

--
-- AUTO_INCREMENT for dumped tables
--

--
-- AUTO_INCREMENT for table `employee_salary`
--
ALTER TABLE `employee_salary`
  MODIFY `id` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=7;
COMMIT;

/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;



