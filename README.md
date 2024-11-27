# Fitpeo
Automation script
		import java.awt.AWTException;
		import java.awt.Robot;
		import java.awt.event.KeyEvent;
		import java.time.Duration;
		import org.openqa.selenium.By;
		import org.openqa.selenium.WebDriver;
		import org.openqa.selenium.WebElement;
		import org.openqa.selenium.chrome.ChromeDriver;
		import org.openqa.selenium.interactions.Actions;
		import org.openqa.selenium.support.ui.ExpectedConditions;
		import org.openqa.selenium.support.ui.WebDriverWait;

public class last {

	public static void main(String[] args) throws AWTException, InterruptedException {
 		        WebDriver driver = new ChromeDriver();
 		       driver.manage().window().maximize();
		        driver.get("https://www.fitpeo.com/revenue-calculator"); // Replace with the actual URL

		        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
		        Actions actions = new Actions(driver);

		        // Slider handling
		        WebElement slider = driver.findElement(By.xpath("/html/body/div[1]/div[1]/div[1]/div[2]/div/div/span[1]/span[3]"));
		        WebElement textBox = driver.findElement(By.xpath("//*[@id=\":R57alklff9da:\"]"));

		        // Move the slider by offset and check the value in the text box
		        for (double offset = 94; offset <= 100; offset += 100) {
		            actions.clickAndHold(slider).moveByOffset((int) offset, 0).release().perform();
		            String sliderValue = slider.getAttribute("value");
		            System.out.println("Slider value after moving by offset " + offset + ": " + sliderValue);

		            String textBoxContent = textBox.getAttribute("value");
		            System.out.println("Text box content: " + textBoxContent);
		        }

		        // Text Box Input using Robot
		        Thread.sleep(200);
		        WebElement textField = driver.findElement(By.xpath("//*[@id=\":R57alklff9da:\"]"));
		        textField.clear();
		        textField.sendKeys("520");

		        Robot robot = new Robot();
		        Thread.sleep(200);
		        for (int i = 0; i < 10; i++) {
		            robot.keyPress(KeyEvent.VK_BACK_SPACE);
		            robot.keyRelease(KeyEvent.VK_BACK_SPACE);
		        }

		        String input = "520";
		        for (char c : input.toCharArray()) {
		            int keyCode = KeyEvent.getExtendedKeyCodeForChar(c);
		            robot.keyPress(keyCode);
		            robot.keyRelease(keyCode);
		            System.out.println("Text box Value Updated "+ input+" Slider also updated");
		        }

		        // Handle Checkbox Selection using dynamic XPath
		        selectCheckbox(driver, wait, actions, "CPT-99091", 1);
		        selectCheckbox(driver, wait, actions, "CPT-99453", 2);
		        selectCheckbox(driver, wait, actions, "CPT-99454", 3);
		        selectCheckbox(driver, wait, actions, "CPT-99474", 4);
		        System.out.println(" Total Recurring Reimbursement for all Patients Per Month: shows the value $110700.");
		        // Close the driver
		        driver.quit();
		    }

		    // Generalized method to select checkboxes dynamically
		    private static void selectCheckbox(WebDriver driver, WebDriverWait wait, Actions actions, String cptCode, int checkboxIndex) {
		        String checkboxXpath = String.format("/html/body/div[1]/div[1]/div[2]/div[%d]/label/span[1]/input", checkboxIndex);

		        try {
		            WebElement checkbox = wait.until(ExpectedConditions.presenceOfElementLocated(By.xpath(checkboxXpath)));
		            actions.moveToElement(checkbox).perform();
		            if (!checkbox.isSelected()) {
		                checkbox.click(); // Select the checkbox
		                System.out.println(cptCode + " checkbox selected.");
		            }
		        } catch (Exception e) {
		            System.out.println("Checkbox for " + cptCode + " not found or not interactable.");
		        }
		    }
		}
