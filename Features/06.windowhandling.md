A **window handle** is a unique ID assigned by the browser to each open window or tab.

`getWindowHandle() `→ Returns the current window ID

`getWindowHandles()` → Returns all open window IDs

## ex:

```
String parent = driver.getWindowHandle();

driver.findElement(By.linkText("Open New Tab")).click();

Set<String> windows = driver.getWindowHandles();

for (String win : windows) {
    if (!win.equals(parent)) {
        driver.switchTo().window(win);
        System.out.println("Child Window Title: " + driver.getTitle());
    }
}

driver.close(); // closes the child window
driver.switchTo().window(parent); // switch back

```

instead of for loop we can use iterator method also 

```
Set<String> windows = driver.getWindowHandles(); //[parentid,childid,subchildId]

Iterator<String>it = windows.iterator();

String parentId = it.next();

String childId = it.next();

driver.switchTo().window(childId);
```

# windowhandling + iframe

```
// Step 1: switch to iframe
driver.switchTo().frame("frameID");

// Step 2: click inside iframe which opens new tab
driver.findElement(By.id("openWindowBtn")).click();

// Step 3: store main window
String mainWindow = driver.getWindowHandle();
Set<String> windows = driver.getWindowHandles();

for (String win : windows) {
    if (!win.equals(mainWindow)) {
        driver.switchTo().window(win);
        System.out.println(driver.getTitle());
        driver.close();
    }
}

// Step 4: back to main window
driver.switchTo().window(mainWindow);

// Step 5: exit iframe
driver.switchTo().defaultContent();
```
