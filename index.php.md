# oop-crud
Employee Management System


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
