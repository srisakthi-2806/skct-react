----------------------Exp-6  Docker Containerization and Docker Volume------------


sudo su
yum install docker
docker --version
systemctl start docker
docker run -it ubuntu bash

ctrl+d--->came out of root

docker volume create my_volume
docker run -d --name my_nginx -v my_volume:/usr/share/nginx/html -p 80:80 nginx
docker inspect my_nginx


-------------------------Exp-7  Docker Networking and Compose for Multi-Container------------


sudo su

yum install docker

sudo systemctl start docker

sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

docker-compose version

nano docker-compose.yml


version: '3.8'

services:
  nginx:
    image: nginx:latest
    ports:
      - "80:80"
    networks:
      - my_network

  mysql:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: example_password
      MYSQL_DATABASE: my_database
      MYSQL_USER: my_user
      MYSQL_PASSWORD: my_password
    networks:
      - my_network

  nodejs:
    image: node:latest
    networks:
      - my_network

networks:
  my_network:
    driver: bridge

ctrl+o--------enter----------ctrl+x


docker-compose up

Ctrl+c

docker ps -a

docker images


--------------------------------Exp-8 cloud9  Banking application-----------------------------

import org.junit.Before;
import org.junit.Test;
import static org.junit.Assert.*;

public class BankTest {
    private BankAccount account;

    @Before
    public void setUp() {
        account = new BankAccount(100); // Initial balance of $100
    }

    @Test
    public void testDeposit() {
        account.deposit(50);
        assertEquals(150, account.getBalance(), 0);
    }

    @Test
    public void testWithdrawSufficientFunds() {
        account.withdraw(50);
        assertEquals(50, account.getBalance(), 0);
    }

    @Test(expected = IllegalArgumentException.class)
    public void testWithdrawExceedsBalance() {
        account.withdraw(200);
    }

    @Test(expected = IllegalArgumentException.class)
    public void testWithdrawNegativeAmount() {
        account.withdraw(-50);
    }

    @Test(expected = IllegalArgumentException.class)
    public void testDepositNegativeAmount() {
        account.deposit(-50);
    }

    @Test
    public void testGetBalance() {
        assertEquals(100, account.getBalance(), 0);
    }
   
    @Test
    public void testDepositAndWithdraw() {
        account.deposit(50);
        account.withdraw(30);
        assertEquals(120, account.getBalance(), 0);
    }
}

class BankAccount {
    private double balance;

    public BankAccount(double initialBalance) {
        this.balance = initialBalance;
    }

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        } else {
            throw new IllegalArgumentException("Deposit amount must be positive");
        }
    }

    public void withdraw(double amount) {
        if (amount > 0 && balance >= amount) {
            balance -= amount;
        } else {
            throw new IllegalArgumentException("Invalid withdrawal amount or insufficient funds");
        }
    }

    public double getBalance() {
        return balance;
    }
}

mkdir -p src

mkdir -p test

cd test

touch BankTest.java

wget https://repo1.maven.org/maven2/org/hamcrest/hamcrest-core/1.3/hamcrest-core-1.3.jar

wget https://repo1.maven.org/maven2/junit/junit/4.13.2/junit-4.13.2.jar

cd ..

mkdir -p out

cd out

touch BankAccount.class

touch BankTest.class

cd ..


javac -cp test/junit-4.13.2.jar:test/hamcrest-core-1.3.jar -d out test/BankTest.java

cd out

java -cp .:../test/junit-4.13.2.jar:../test/hamcrest-core-1.3.jar org.junit.runner.JUnitCore BankTest






















/*-----------------------------exp-1 Employee Management System-------------------------------------------- */

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement; 
public class App { 
    static final String JDBC_Driver="com.mysql.cj.jdbc.Driver";
    static final String Db_url="jdbc:mysql://localhost/employee_management";
    static final String USER="root";
    static final String PASS="root";
public static void main(String[] args) throws Exception {
    Connection conn = null;
    Statement stmt = null;
    try{
    Class.forName(JDBC_Driver);
    conn = DriverManager.getConnection(Db_url, USER, PASS);
    if(conn!=null){
        System.out.println("Connected to the Database!");
        conn.setAutoCommit(false);
        stmt = conn.createStatement();
 // CREATE OPERATION
    String createUserSQL ="INSERT INTO employee(e_id,e_name,e_email,e_phno,e_salary) VALUES (1,'Sri', 'sri@gmail.com','9750757390',100000),(2,'Pri','Pri@gamil.com','9080508557',80000),(3,'kavi', 'kavi@gmail.com','8508997859',90000)";
    int rowsInserted = stmt.executeUpdate(createUserSQL);
    System.out.println(rowsInserted + " row(s) inserted.");
 //READ OPERATION
    String readUserSQL = "SELECT * FROM employee";
    ResultSet rs = stmt.executeQuery(readUserSQL);
    while(rs.next()){
        int id=rs.getInt("e_id");
        String name=rs.getString("e_name");
        String email=rs.getString("e_email");
        String phno=rs.getString("e_phno");
        int salary=rs.getInt("e_salary");
        System.out.println("ID: "+id+",Name: "+name+",Email: "+email+",Phone_No: "+phno+",Salary: "+salary);
                }
    rs.close();
    //UPDATE OPERATION
        String updateUserSQL = "UPDATE employee SET e_salary='200000' WHERE e_name='Sri'";
        int rowsUpdated = stmt.executeUpdate(updateUserSQL);
        System.out.println(rowsUpdated + "row(s) updated.");

            

    //DELETE OPERATION
        String deleteUserSQL = "DELETE FROM employee WHERE e_name='Sri'";
        int rowsDeleted = stmt.executeUpdate(deleteUserSQL);
        System.out.println(rowsDeleted + "row(s) deleted.");
        conn.commit();
    
            }
            
    else{
        System.out.println("Failed to make connection!");
        }
    }
    catch(SQLException se){
    se.printStackTrace();
    try{
    if(conn!=null){
    conn.rollback();
    }
}
    catch (SQLException e) {
    e.printStackTrace();
    } }
    finally { try {
        if (stmt != null) {
        stmt.close();
    }
        if (conn != null) {
        conn.close();
        }
    } 
    catch (SQLException e) {
    e.printStackTrace();
    } } }}

-----DATABASE QUERY-----
create table employee (e_id int,
e_name varchar(30),
e_email varchar(30),
e_phno int,
e_salary int );
select * from employee;
alter table employee modify column e_phno varchar(30);
select *from employee;




/*--------------------------------------exp-2 Vehicle Management------------------------------------------ */

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

public class App {
    static final String JDBC_Driver="com.mysql.cj.jdbc.Driver";
    static final String Db_url="jdbc:mysql://localhost/Vehiclejdbc";
    static final String USER="root";
    static final String PASS= "root";
    public static void main(String[] args) throws Exception {
        
        Connection conn =null;
        Statement stmt =null;

        try{
            Class.forName(JDBC_Driver);
            conn =DriverManager.getConnection(Db_url,USER,PASS);

            if(conn != null)
            {
                System.out.println("Connected to the Database..!");
                conn.setAutoCommit(false);
                stmt =conn.createStatement();
                //INSERT
                String create ="INSERT INTO vehicle_management VALUES('1','Toyota','Camry','Silver','2022','Petrol',650000),('2','Honda','Civic','Black','2020','Hybrid',1200000),('3','Toyota','Camry','red','2000','Disel',850000),('4','TATA','Civic','Grey','2019','Petrol',240000)";
                int rowsInserted =stmt.executeUpdate(create);
                System.out.println(rowsInserted +"rows inserted.");
            

            //READ
            String read="SELECT * FROM vehicle_management";
            ResultSet rs = stmt.executeQuery(read);
            while(rs.next())
            {
                int id =rs.getInt("id");
                String  manufacture=rs.getString("manufacture");
                String  model=rs.getString("model");
                String  color=rs.getString("color");
                String  manufactureYear=rs.getString("manufactureYear");
                String  fuel=rs.getString("fuel");
                String  price=rs.getString("price");

                System.out.println("Id: "+ id + "\nManufacture: " + manufacture+"\nModel: "+model+"\nColor: "+color+"\nManufacture year: "+manufactureYear+"\nFuel: "+fuel+"\nPrice: "+price+"\n");

            }
            rs.close();

            //UPDATE
            String update ="UPDATE vehicle_management SET price='600000' WHERE manufacture='Toyota'";
            int rowsUpdated =stmt.executeUpdate(update);
            System.out.println(rowsUpdated + " rows Updated.");

            //DELETE
            String delete ="DELETE from vehicle_management WHERE id='1'";
            int rowsDeleted =stmt.executeUpdate(delete);
            System.out.println(rowsDeleted +" rows Deleted.");
            conn.commit();
        }
        
        else
        {
            System.out.println("Failed to make Connection");
        }
    
    }
        catch(Exception e)
        {
            System.out.println(e.getMessage().toString());
        }
    }
}

--------DATABASE QUERY---------
create database Vehiclejdbc;
use Vehiclejdbc;
create table vehicle_management(
id int,
manufacture varchar(30),
model varchar(30),
color varchar(20),
manufactureYear varchar(20),
fuel varchar(20),
price varchar(30)
);
select *from vehicle_management;


/*----------------------------------exp-3 FaceBook Signup----------------------------------------------- */

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.Select;

public class App {
public static void main(String[] args) throws Exception {
    System.setProperty("webdriver.chrome.driver","C:\\Users\\srisa\\Desktop\\AutoTesting\\Exp3\\Exp3\\src\\Driver\\chromedriver-win64\\chromedriver-win64\\chromedriver.exe");
    WebDriver driver = new ChromeDriver();
    driver.get("https://www.facebook.com/signup");
    driver.manage().window().maximize();

    WebElement firstname = driver.findElement(By.name("firstname"));
    WebElement lastname = driver.findElement(By.name("lastname"));
    WebElement mobileno = driver.findElement(By.name("reg_email__"));
    WebElement password = driver.findElement(By.name("reg_passwd__"));
    WebElement Date = driver.findElement(By.name("birthday_day"));
    WebElement month = driver.findElement(By.name("birthday_month"));
    WebElement year = driver.findElement(By.name("birthday_year"));
    WebElement gender = driver.findElement(By.xpath("//input[@value='-1']"));
    WebElement pronoun = driver.findElement(By.name("preferred_pronoun"));
    WebElement submit = driver.findElement(By.name("websubmit"));

    firstname.sendKeys("Sri");
    lastname.sendKeys("S");
    mobileno.sendKeys("7967562390");
    password.sendKeys("sri@2004");
    Date.sendKeys("21");
    month.sendKeys("Nov");
    year.sendKeys("2004");
    if (!gender.isSelected()) {
        gender.click();
    }

        Select pronouns = new Select(pronoun);
        pronouns.selectByVisibleText("She: \"Wish her a happy birthday!\"");
        submit.click();
        Thread.sleep(10000000);
        driver.quit();
    }
}




/*----------------------------------epx-4 Coursera Login----------------------------------------------------- */
import java.time.Duration;
import org.openqa.selenium.By;
import org.openqa.selenium.Keys;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;



public class App {
public static void main(String[] args) throws InterruptedException {
    System.setProperty("webdriver.chrome.driver","C:\\Users\\srisa\\Desktop\\AutoTesting\\Exp4\\Exp4\\src\\Driver\\chromedriver-win64\\chromedriver-win64\\chromedriver.exe");
    WebDriver driver = new ChromeDriver();
    driver.get("https://www.coursera.org/?authMode=login");
    driver.manage().window().maximize();

// Email Field (id)
    WebElement emailInput = driver.findElement(By.id("email"));
    emailInput.sendKeys("727822tuit231@skct.edu.in");

// Password Field (name)
    WebElement passwordInput = driver.findElement(By.name("password"));
    passwordInput.sendKeys("$ri$@kthi2806");

// Mouse Handling for click login button (xpath)
    WebDriverWait login = new WebDriverWait(driver, Duration.ofSeconds(20));
    WebElement clickLogin =login.until(ExpectedConditions.elementToBeClickable(By.xpath("/html/body/div[5]/div/div/section/section/div[1]/form/div[3]/button")));
    clickLogin.click();

    Thread.sleep(30000);

// Search box (xpath)
    WebElement search = driver.findElement(By.xpath("/html/body/div[2]/div/div[2]/span/div[1]/header/div[1]/div/di v/ div[2]/div/div/div[1]/div/div/div[3]/div/div[2]/form/div/div[1]/input"));
    search.sendKeys("Testing methodologies");

// Keyboard selector for enter search button
Actions actions = new Actions(driver);
actions.sendKeys(Keys.ENTER).perform();
}
}



/*-----------------------------exp-5 LinkedIn login-------------------------------------------- */
import org.openqa.selenium.By;
import org.openqa.selenium.Keys;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;

public class App {
public static void main(String[] args) throws Exception {
    System.setProperty("webdriver.chrome.driver","C:\\Users\\srisa\\Desktop\\AutoTesting\\Exp5\\Exp5\\src\\Driver\\chromedriver-win64\\chromedriver-win64\\chromedriver.exe");
    WebDriver driver=new ChromeDriver();  driver.get("https://www.linkedin.com/");
    driver.manage().window().maximize();

    WebElement username = driver.findElement(By.id("session_key"));
    WebElement password = driver.findElement(By.id("session_password"));
    username.sendKeys("22it.srisakthi.s@skct.edu.in");
    password.sendKeys("SriSakthi2806");

    WebElement sub = driver.findElement(By.xpath("//*[@id=\"main-content\"]/section[1]/div/div/form/div[2]/button"));
    sub.click();
    Thread.sleep(300);

//For typing the Job
    WebElement search = driver.findElement(By.xpath("/html/body/div[5]/header/div/div/div/div[1]/input"));
    search.sendKeys("Reactjs Developer jobs");
    Actions actions = new Actions(driver);
    actions.sendKeys(Keys.ENTER).perform();
    Thread.sleep(3000);

    WebElement seeAllJob = driver.findElement(By.xpath("//*[@id=\"H8L+gDJyRQqZadvN0gIuuQ==\"]/div/div[2]/a"));
    seeAllJob.click();
    Thread.sleep(300);

//for Selecting the paticular Job from search result
WebElement clickApply = driver.findElement(By.xpath("/html/body/div[5]/div[3]/div[4]/div/div/main/div/div[2]/div[1]/div/ul/li[1]/div/div/div[1]/div[1]/div[2]/div[1]/a/strong"));
clickApply.click();
Thread.sleep(300);

//After selecting the job click apply
WebElement clickApplyy = driver.findElement(By.xpath("/html/body/div[5]/div[3]/div[4]/div/div/main/div/div[2]/div[2]/div/div[2]/div/div[1]/div/div[1]/div/div[1]/div[1]/div[5]/div/div/div/button/span"));
clickApplyy.click();

//After selecting the job click Mobile Number
WebElement mobilenumber = driver.findElement(By.xpath("/html/body/div[3]/div/div/div[2]/div/div[2]/form/div/div/div[4]/div/div/div[1]/div/input"));
mobilenumber.sendKeys("7418529631");
Thread.sleep(4000);

//After selecting the job click Next Button
WebElement nextButton = driver.findElement(By.xpath("/html/body/div[3]/div/div/div[2]/div/div[2]/form/footer/div[2]/button"));
nextButton.click();
}
}



/*-------------------------exp-6 Hospital Appointment Booking------------------------------- */

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

public class App {
public static void main(String[] args) throws Exception{
    System.setProperty("webdriver.chrome.driver","C:\\Users\\srisa\\Desktop\\AutoTesting\\Exp6\\Exp6\\src\\Driver\\chromedriver-win64\\chromedriver-win64\\chromedriver.exe");
    WebDriver driver = new ChromeDriver();
    driver.get("https://www.psghospitals.com/book-an-appointment/#dr_appointment");
    driver.manage().window().maximize();

    WebElement doctor = driver.findElement(By.xpath("//*[@id='Container']/div[1]/div[1]/a/img[2]"));
    doctor.click();
    Thread.sleep(100);

    WebElement doctorappoin = driver.findElement(By.xpath("/html/body/div[7]/div/div/div/div/div/div/div/div/div[2]/div/div[2]/ul/li[1]/a"));
    doctorappoin.click();
    WebElement name = driver.findElement(By.name("PatientName"));
    name.sendKeys("sri");
    WebElement email = driver.findElement(By.name("Email"));
    email.sendKeys("sri@gmail.com");
    WebElement num = driver.findElement(By.name("Mobile"));
    num.sendKeys("1234567890");

//Preferreddate
    WebElement date = driver.findElement(By.name("Preferreddate"));
    date.sendKeys("22-09-2024");
    WebElement det = driver.findElement(By.name("OtherDetails"));
    det.sendKeys("nil");
    WebElement submit = driver.findElement(By.xpath("/html/body/div[5]/div/div/div/div/div/div/div/form/ul/li[10]/p/input"));
    submit.click();
    //OtherDetails
}
}



/*-----------------------exp-7 Mouse and Keyboard events ebay website------------------------ */

import org.openqa.selenium.By;
import org.openqa.selenium.Keys;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;

public class App {
public static void main(String[] args) throws Exception {
    System.setProperty("webdriver.chrome.driver","C:\\Users\\srisa\\Desktop\\AutoTesting\\Exp7\\Exp7\\src\\Driver\\chromedriver-win64\\chromedriver-win64\\chromedriver.exe");
    WebDriver driver = new ChromeDriver();
    driver.get("https://www.ebay.com/");
    driver.manage().window().maximize();

    Actions action = new Actions(driver);
    WebElement collections = driver.findElement(By.xpath("//*[@id='vl-flyout-nav']/ul/li[5]")); 
    action.moveToElement(collections).click().perform();

    WebElement comics = driver.findElement(By.xpath("//*[@id='mainContent']/section[2]/div[2]/span[1]/a/div[1]/img"));
    action.sendKeys(Keys.ARROW_DOWN).perform();
    action.moveToElement(comics).perform();
    Thread.sleep(3000);
    comics.click();

    WebElement bookTitle = driver.findElement(By.xpath("//*[@id=\"mainContent\"]/section[1]/div[2]/span[4]/a/div[1]/img"));
    action.sendKeys(Keys.ARROW_DOWN).perform();
    action.moveToElement(bookTitle).perform();
    Thread.sleep(3000);
    bookTitle.click();

    WebElement marvel = driver.findElement(By.xpath("//*[@id=\"s0-28-9-0-1[0]-0-0-4list\"]/li[1]/a"));
    action.moveToElement(marvel).perform();
    Thread.sleep(3000);
    marvel.click();

    WebElement search = driver.findElement(By.xpath("//*[@id=\"gh-ac\"]"));
    search.sendKeys("decors");
    Thread.sleep(2000);
    action.sendKeys(Keys.BACK_SPACE).perform();
    Thread.sleep(2000);
    search.sendKeys(" items");
    action.sendKeys(Keys.ENTER).perform();
    Thread.sleep(5000);

    driver.quit();
}
}


/*-------------------------------exp-8 FilpKart--------------------------------------------------- */
import java.time.Duration;
import org.openqa.selenium.By;
import org.openqa.selenium.Keys;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

public class App {
public static void main(String args[]) throws InterruptedException {
    System.setProperty("webdriver.chrome.driver","C:\\Users\\srisa\\Desktop\\AutoTesting\\Exp8\\Exp8\\src\\Driver\\chromedriver-win64\\chromedriver-win64\\chromedriver.exe");
    WebDriver driver=new ChromeDriver();
    driver.get("https://www.flipkart.com");
    driver.manage().window().maximize();

    WebElement searchBox = driver.findElement(By.xpath("//*[@id=\"container\"]/div/div[1]/div/div/div/div/div[1]/div/div[1]/div/div[1]/div[1]/header/div[1]/div[2]/form/div/div/input"));
    searchBox.sendKeys("Headphones");
    Actions action = new Actions(driver);
    action.sendKeys(Keys.ENTER).perform();
    Thread.sleep(500);

    //String xpathExpression;
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(20));
    WebElement searchResult = wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//*[@id=\"container\"]/div/div[3]/div[1]/div[2]/div[2]/div/div[1]/div/a[1]/div[1]/div/div/img")));

    //WebElement searchResult = driver.findElement(By.xpath("//*[@id=\"search\"]/d iv[1]/div[1]/div/span[1]/div[1]/div[3]/div/div/div/div/span/div/div/div/div[2]/div/div/div[1]/h2/a/span"));  
    searchResult.click();


    WebElement buyNowButton = wait.until(ExpectedConditions.elementToBeClickable(By.className("abutt on-input")));
    WebElement apply = wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//*[@id=\"container\"]/div/div[3]/div[1]/div[2]/div[2]/div/div[1]/div/a[2]")));
    apply.click();

driver.quit();
}
}


/*----------------------------exp-9 Amazon Website-------------------------------- */


import java.util.concurrent.TimeUnit;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

public class App {
@SuppressWarnings("deprecation")
public static void main(String[] args) throws InterruptedException {
    System.setProperty("webdriver.chrome.driver", "C:\\Users\\srisa\\Desktop\\AutoTesting\\Exp9\\Exp9\\src\\Driver\\chromedriver-win64\\chromedriver-win64\\chromedriver.exe");
    WebDriver driver = new ChromeDriver();
    driver.manage().window().maximize();
    driver.get("https://www.amazon.com/home");
    Thread.sleep(1000);

// Search for an item
    WebElement searchBox = driver.findElement(By.id("twotabsearchtextbox"));
    searchBox.sendKeys("laptop");
    searchBox.submit();

// Click on the first search result
    WebElement firstSearchResult = driver.findElement(By.xpath("/html/body/div[1]/div[1]/div[1]/div[1]/div/span[1]/div[1]/div[3]/div/div/div/div/span/div/div/div/div[2]/div/div/div[4]/div[1]/div/div[3]/div/div/div[1]/span/div/span/span/button"));
    firstSearchResult.click();

// Set implicit wait
    driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);

// Add the item to cart
    WebElement addToCartButton = driver.findElement(By.xpath("//input[@id='add- to-cartbutton']"));
    addToCartButton.click();
    

// Navigate back to the homepage
    driver.navigate().back();

// Search for another item
    searchBox = driver.findElement(By.id("twotabsearchtextbox"));
    searchBox.clear();
    searchBox.sendKeys("headphones");
    searchBox.submit();

// Click on the first search result for headphones
    WebElement firstHeadphonesResult = driver.findElement(By.xpath("(//span[@class='a-sizemedium a-color-base a-text- normal'])[1]"));
    firstHeadphonesResult.click();

    Thread.sleep(10000); driver.quit();
}
}



/*---------------------------exp-10 login Testing---------------------------------------- */
import org.testng.Assert;
import org.testng.annotations.Test;
public class App {
//Check The Email Validation
@Test(groups = {"authentication"})
public void testValidEmail() {
boolean emailResult = performEmailValidation("validEmail");
Assert.assertEquals(emailResult, true, "Email is Valid");
}
//Check The Phone Number Validation
@Test(groups = {"authentication"})
public void testValidPhoneNumber() {
boolean loginPhone = performPhoneValidation( 123456789L);
Assert.assertEquals(loginPhone, true, "Phone Number is valid");
}
//Check Login Credentials Both Username And Password
@Test(groups = {"authentication"})
public void testLogin() {
boolean loginResult = performLoginBoth("validUsername", "validPassword");
Assert.assertEquals(loginResult, true, "Login Successfull");
}
//Check OTP Verification
@Test(groups = {"authentication"})
public void testOTP() {
boolean loginOTP = performValidOTP(0223);
Assert.assertEquals(loginOTP, true, "OTP is Valid");
}
private boolean performEmailValidation(String email) {
if(email.equals("validEmail")) return true;
return false;
}
private boolean performPhoneValidation(long phone) {
if(phone == 123456789)
return true;
return false;
}
private boolean performLoginBoth(String username, String password) {
if(username.equals("validUsername") && password.equals("validPassword"))
return true;
return false;
}
private boolean performValidOTP(int otp) {
if(otp == 0223) return true; return false;
}
}

/*---------------------------exp-10 login Testing(anoter code)---------------------------------------- */

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.annotations.AfterClass;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;


public class App {

    private WebDriver driver;
@BeforeClass
public void setUp()
{
    System.setProperty("webdriver.chrome.driver","C:\\Users\\srisa\\Desktop\\AutoTesting\\Exp10\\Exp10\\src\\Driver\\chromedriver-win64\\chromedriver-win64\\chromedriver.exe");
    driver=new ChromeDriver();
    driver.get("https://the-internet.herokuapp.com/login");
    driver.manage().window().maximize();
    
}


@Test
public void  uname()
{
    WebElement name = driver.findElement(By.name("username"));
    name.sendKeys("Sri Sakthi");
}
@Test
public void  pswrd()
{
    WebElement password = driver.findElement(By.name("password"));
    password.sendKeys("2806");
}
@Test
public void submit()
{
    System.out.println("Failure Test case");
    WebElement sub = driver.findElement(By.xpath("/html/body/div[2]/div/div/form/button/i"));
    sub.click();
}

@AfterClass
public void tearDown(){
    if(driver != null)
    {
        driver.quit();
    }
}


}

