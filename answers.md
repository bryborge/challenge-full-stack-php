# My Answers

1.  **What is an IP subnet? What kind of notation would you use to express them?**

    An IP subnet is a logically allocated "subsect" of network addresses on a given network. Subnets can be bigger or
    smaller based on the connectivity needs of the person or company, and it can improve network performance by
    sectioning off certain IP ranges for different things. For example, a coffee shop might want to have three
    subnets -- one for the office, one for the POS system, and one for patrons at the coffee shop. Each subnet would
    have a range of usable IP addresses.

    You can express an IP subnet in binary notation (if you're a genius), or you can decimal notation, which makes
    better sense for most humans. :)

2.  **Explain the differences between HEAD, GET and POST in the HTTP protocol.**

    The GET method is used to *get* data from a resource. The POST method is used to create or update some data on a
    resource. Finally, the HEAD method is used to get the headers that are returned from a resource.  It's almost the
    same as a GET request except that it doesn't return the body of the response.  That's useful if you want to know
    what you'll get from a GET request before actually making the request.

    Another thing to note is that GET requests are cacheable whereas POST requests are not.

3.  **What is a certificate and how does it relate to HTTPS? What advantages does a CA signed certificate offer over a**
    **self-signed certificate (be specific)? What security benefits does HTTPS provide?**

    Certificates are used to provide encryption, and with respect to HTTPS, they are used to encrypt data as it's being
    passed back and forth in requests.  With standard HTTP, one could intercept the data in transit, change it, and then
    allow it to continue to its destination.  In terms of student data, for example, that would mean a student could
    potentially change their grades or all other sorts of nefarious things.

    CA refers to "certificate authority" and are used to prove the authenticity of the transactions made between
    services. This is different from a self-signed certificate, which is issued by the person using it. They both offer
    encryption but the key difference is that CA certs "prove" authenticity by only being issued by authorities that are
    permitted to do so.

    In sum, CA certs are used for internet-facing systems, and self-signed certs are perfectly fine for most intranets
    and local use.

4.  **Convert the following procedural PHP7 code into object oriented style code.**

    ```php
    function make_astronaut( string $name, float $weight ) {
        return [ 'name' => $name, 'weight' => $weight ];
    }

    function add_weight_to_astronaut( array &$astronaut, float $pounds ) {
        $astronaut['weight'] += $pounds;
    }

    function launch_astronaut( array $astronaut ) {
        if ( $astronaut['weight'] > 200 ) {
            echo "{$astronaut['name']} too heavy, grounded.\n";
        } else {
            echo "{$astronaut['name']} going where no human has gone before.\n";
        }
    }
    ```

    ```php
    /**
     * Astronaut
     *
     * Example Use:
     *
     * $neil = new Astronaut('Neil Armstrong', 180.3);
     * $neil->add_weight_to_astronaut(20.2);
     * $neil->launch_astronaut();
     *
     * > Neil Armstrong is too heavy, grounded.
     */
    class Astronaut
    {
        private $name;
        private $weight;

        function __construct($name, $weight)
        {
            $this->name = $name;
            $this->weight = $weight;
        }

        function set_name($name)
        {
            $this->name = $name;
        }

        function set_weight($weight)
        {
            $this->weight = $weight;
        }

        function get_name()
        {
            return $this->name;
        }

        function get_weight()
        {
            return $this->weight;
        }

        function add_weight_to_astronaut($pounds)
        {
            $this->weight += $pounds;
        }

        function launch_astronaut()
        {
            if ($this->weight > 200) {
            echo "{$this->name} is too heavy, grounded.\n";
            } else {
            echo "{$this->name} is going where no human has gone before.\n";
            }
        }
    }
    ```

5.  **You are given the following table in database `nasa`:**

    `CREATE TABLE astronaut ( name text, weight int );`

    Write a simple PHP page that displays a form to enter the name and weight of an astronaut and on submission enters
    those values into the database. (Simple in this case means you do not have to be fancy, but you still need to be
    complete and correct. Assume the database is running on the local host and use user nasa and password nasa to
    connect). Use a DB abstraction layer to write your code. Example: PDO or WPDB. Use prepared statements.

    ```php
    <!DOCTYPE html>
    <html lang="en" dir="ltr">
    <head>
        <meta charset="utf-8">
        <title></title>
    </head>
    <body>
        <form action="index.php" method="post">
        <label for="name">Name:
            <input type="text" name="name" id="name" />
        </label>
        <label for="weight">Weight:
            <input type="text" name="weight" id="weight" />
        </label>
        <button type="submit" name="submit">Submit</button>
        </form>
    </body>
    </html>

    <?php
    if (isset($_POST['submit'])) {
        $host = "localhost";
        $db = "nasa";
        $username = "nasa";
        $password = "nasa";
        $charset = 'utf8mb4';

        $dsn = "mysql:host=$host;dbname=$db;charset=$charset;";
        $options = [
            PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
            PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
            PDO::ATTR_EMULATE_PREPARES => false,
        ];

        // connect to db
        try {
            $conn = new PDO($dsn, $username, $password, $options);

            $name = $_POST['name'];
            $weight = $_POST['weight'];

            // add astronaut to db on form submit
            $stmt = $conn->prepare("INSERT INTO astronaut (name, weight) VALUES (:name, :weight)");
            $stmt->bindParam(':name', $name);
            $stmt->bindParam(':weight', $weight);
            $stmt->execute();

            echo "Success: New astronaut added to database!";

            // close db connection
            $conn = null;
        } catch(\PDOException $e) {
            throw new \PDOException($e->getMessage(), $e->getCode());
        }
    }
    ```

6.  **You have a group of people that are taking courses from a given list of courses. Write ANSI compliant SQL**
    **statements that will do the following:**

    *   Create the tables that allow any person to take any course, but only allow them to sign up for any given course
        once.
    *   Show all courses taken by a given person.
    *   Show all people and the number of courses they are taking.

    ```sql
    -- Create tables, and only allow students to enroll in a course once
    CREATE TABLE student (
        id int NOT NULL AUTO_INCREMENT PRIMARY KEY,
        last_name varchar(255),
        first_name varchar(255)
    );

    CREATE TABLE course (
        id int NOT NULL AUTO_INCREMENT PRIMARY KEY,
        course_name varchar(255),
        course_prefix varchar(255),
        course_number int,
        course_description text
    );

    CREATE TABLE student_course (
        id int NOT NULL AUTO_INCREMENT PRIMARY KEY,
        student_id int NOT NULL,
        course_id int NOT NULL,
        UNIQUE(student_id, course_id)
    );

    -- Show all courses taken by student with id of '1'
    SELECT course.* FROM student
        JOIN student_course ON (student.id = student_course.student_id)
        JOIN course ON (student_course.course_id = course.id)
        WHERE student.id = 1;

    -- Show all students and the number of courses they're taking
    SELECT student.first_name, student.last_name, COUNT(student_course.student_id) AS courses_taken
        FROM student_course
        JOIN student ON student.id = student_course.student_id
        GROUP BY student_id;
    ```

7.  **Given the following string:**

    ```php
    $str = 'drinking giving jogging 喝 喝 passing 制图 giving 跑步 吃';
    ```

    Using PHP, write a function that:

    1.  Moves all Chinese characters to the end of the string, reversing their current order.
    2.  Removes duplicate English words.
    3.  Returns the modified string.

    ```php
    /**
     * Filters all chinese words to the end of the string and reverses their order, and removes duplicate words.
     *
     * @param  string $str The string to be modified
     * @return string      The modified string
     */
    function modify_string( string $str ) {
        $uniques = array_unique( explode( ' ', $str ) );
        $chinese = [];
        $english = [];

        foreach( $uniques as $word ) {
            if ( ! preg_match( '/[A-Za-z]/', $word ) ) {
            array_push( $chinese, $word );
            } else {
            array_push( $english, $word );
            }
        }

        $chinese = array_reverse( $chinese );

        return implode( ' ', array_merge( $english, $chinese ) );
    }
    ```

8.  **Use a GitHub Branch as a Composer Dependency**

    Explain how you would configure composer to use `https://github.com/peanut-butter/new-private-project` with the
    branch `bugfixes` in your project.

    ```php
    {
        "repositories": [
            {
            "type": "git",
            "url": "https://github.com/peanut-butter/new-private-project"
            }
        ],
        "require": {
            "peanut-butter/new-private-project": "dev-hotfix"
        }
    }
    ```

9.  **Get "peanut-butter" working locally and attach a screenshot showing that you could do it. If there are problems,**
    **document and fix them.**

    This answer has been omitted.

10. **This string contains information about this test. Can you determine what its significance is?**

    ```php
    'aXA6MTA0LjIwMC4xMzIuMTg0IHRpbWU6MjAxOS0xMC0wNCAxNTo1MzoxMw=='
    ```

    That is a base64 encoded string that, when decrypted, reads:

    `ip:104.200.132.184 time:2019-10-04 15:53:13`

    Which is the IP address from which I accessed the programmer_test.txt file, and the time at which I did it (EST).