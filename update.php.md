# oop-crud
Employee Management System

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
