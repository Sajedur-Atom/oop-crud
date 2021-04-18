# oop-crud
Employee Management System

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
