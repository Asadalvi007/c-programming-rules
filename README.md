# C Programming Style Guide

## 1. C Language

### Rule 1.1 – Use C99 Standard
- You can use GNU99 mode if you require extended C features such as `atoff()`.

### Rule 1.2 – Use C Standard Libraries
Instead of writing code from scratch, make use of C Standard libraries. Here are some commonly used libraries:

- `<complex.h>`: Complex number arithmetic
- `<ctype.h>`: Character type checking
- `<limits.h>`: Sizes of basic types
- `<math.h>`: Common mathematical functions
- `<stdarg.h>`: Variable arguments capture
- `<stdbool.h>`: Boolean type
- `<stdint.h>`: Fixed-width integer types
- `<stdio.h>`: Input/output
- `<stdlib.h>`: General utilities (memory, program, string conversions, random, and algorithms)
- `<string.h>`: String handling
- `<time.h>`: Time and date utilities

---

## 2. Code Formatting

### Indentation
- Avoid using the tab (`\t`) character for indentation.
- Use spaces only for indentation.
- Indentation size: **2 spaces**
- Apply indentation for:
  - Statements within function body
  - Statements within block
  - Statements within switch body
  - Statements within case body
  - `break` statements
  - Labels

### Braces
- Place braces on the next line for:
  - Function declaration
  - Blocks
  - Case statements in `switch`
  - `switch` statement
  - Linkage (`extern`)

### White Space
- Add white space for declarator lists after commas and after pointers.
- For functions:
  - No white space before pointer
  - Add white space after pointer

    Example:
    ```c
    void updateValue (int* pValue);
    ```
- Add white space:
  - Before opening parentheses for function calls
  - After semicolons in `for` loop parameters
  - After commas in function arguments
  - Before and after assignment and binary operators
  - After commas in initializer lists

### Control Statements
- Insert a new line before `else` in an `if` statement.
- Do not insert a new line before `while` in a `do-while` statement.
- Keep `else if` on the same line.

### Line Wrapping
- Recommended maximum line width: **120 characters**.
- Indent wrapped lines with **2 levels** of indentation.
- Guidelines for wrapping:
  - Wrap all elements of a function on new lines.
  - Always wrap `enum` lists, regardless of line width.
  - Wrap only necessary parts of expressions and initializer lists.

---

## 3. Variables

### Rule 3.1 – Use camelCase Naming Convention
- Use camelCase for variable names:
  - First word starts with lowercase, subsequent words with uppercase.
  
    Example:
    ```c
    int16_t initialVerticalSpeed;
    float gpsAverageAltitude;
    ```

### Rule 3.2 – Use Clear and Descriptive Names
- Avoid abbreviations. Use short but descriptive names.

    Example:
    ```c
    int16_t avgAlt;           // (Bad)
    int16_t averageAltitude;  // (Good)
    ```

### Rule 3.3 – Declare Variables in Separate Lines
- Improves readability and reduces bugs.

    Example:
    ```c
    uint32_t startTime, finalTime, deltaTime;  // (Bad)
    uint32_t startTime;    // (Good)
    uint32_t finalTime;
    uint32_t deltaTime;
    ```

- Align variable names and values for better readability:

    Example:
    ```c
    uint16_t totalGreenApples = 15;
    uint16_t totalRedApples   = 17;
    float    verticalSpeed    = 12.56f;
    ```

### Rule 3.4 – Pointer Variable Prefix
- Use the letter **p** as a prefix for pointer variables.

    Example:
    ```c
    char* pStudentName;
    ```

### Rule 3.5 – Double Pointer Variable Prefix
- Use **pp** as a prefix for double pointer variables.

    Example:
    ```c
    char** ppWifiNames;
    ```

### Rule 3.6 – Global Variable Prefix
- Use **g** as a prefix for global variables.

    Example:
    ```c
    static uint8_t gNextIndex;
    ```

### Rule 3.7 – Combined Global and Pointer Prefix
- For global pointer variables, use both **g** and **p** as prefixes.

    Example:
    ```c
    static char* gLastName;
    ```

### Rule 3.8 – Use `static` for Variables with File-Level Scope
- Use `static` for variables visible only within a source file, but avoid using `static` inside a function.

### Rule 3.9 – Use `volatile` for Variables Modified by Interrupts or Hardware
- Declare variables as `volatile` if modified by an interrupt or if they are memory-mapped pointers.

    Example (Tick Counter):
    ```c
    volatile uint32_t gCounterTicks = 0;
    
    void isrIncrementCounter(void)
    {
      gCounterTicks++;
    }

    uint32_t getCounter(void)
    {
      return gCounterTicks;
    }
    ```

    Example (Memory-Mapped Pointer):
    ```c
    volatile uint8_t* pInputSwitch = (uint8_t*)(GPIOA->IDR);

    while (!(*pInputSwitch & 0x01)); // Wait for switch activation
    printf("Switch is Activated!");
    ```

### Rule 3.10 – Use `typedef` for Application Variables
- Use `typedef` for application-specific variables to make future changes easier.
  
    Example:
    ```c
    typedef uint32_t GpsLatitude_Micro_t;
    typedef float    AverageSpeed_Kmh_t;
    typedef uint32_t Distance_Meters_t;
    ```

---

## 4. Const Function Parameters

### Rule 4.1 – Constant Value Parameter
- Use `const` keyword for constant value parameters.

    Example:
    ```c
    void setSpeed(const uint16_t speed)
    {
      gMotorConfig->forwardSpeed = speed;
    }
    ```

### Rule 4.2 – Constant Pointer Parameter
- Use `const` keyword for constant pointers.

    Example:
    ```c
    void updateWifiNameTo4G(WifiName_t* const pWifi)
    {
      sprintf(pWifi->buf, "%s - 4G", pWifi->buf);
    }
    ```

### Rule 4.3 – Both Constant Value and Constant Pointer Parameter
- Use `const` keyword for both constant value and pointer.

    Example:
    ```c
    void printWifiName(const WifiName_t* const pWifi)
    {
      printf("Selected Wifi Name = %s\n", pWifi->buf);
    }
    ```

## 5. Structure
### Rule 5.1 – Always typedef a structure
```c
        typedef struct
        {
        …
        }
```
### Rule 5.2 – Use PascalCase for Structure name
```c
        typedef struct
        {
        …
        }AccelData_t;
```
- Typedef variable name still uses camelCase
AccelData_t accelData;

### Rule 5.3 – Add the suffix _t to the typedef name
```c
    typedef struct
    {
    int16_t accelX;
    int16_t accelY;
    int16_t accelZ;
    bool    isValid;
    }AccelData_t;
```
### Rule 5.4 – Use filename as a prefix to the struct typedef name
```c
typedef struct
{
  int16_t accelX;
  int16_t accelY;
  int16_t accelZ;
  bool    isValid;
}Mpu6050_Accel_t;
```
### Rule 5.5 – Do not memory-map structure bitfields, instead access them directly
```c
typedef struct
{
  uint8_t reserved : 3; //LSb
  uint8_t afs_sel : 2;
  uint8_t za_st : 1;
  uint8_t ya_st : 1;
  uint8_t xa_st : 1;    //MSb
}Mpu6050_AccelConfigReg_t;

int main (void)
{
  Mpu6050_AccelConfigReg_t hAccelConfig;
  hAccelConfig.xa_st = 0;
  hAccelConfig.ya_st = 1;
  hAccelConfig.za_st = 0;
  hAccelConfig.afs_sel = 2;
  hAccelConfig.reserved = 0;

  uint8_t accelConfigBits = *(uint8_t *)&hAccelConfig;

  getchar();
  return 0;
}
```
## 6.	Enums
### Rule 6.1 – Always typedef an Enum
```c
typedef enum
{
…
};
```
### Rule 6.2 – Use PascalCase for Enum Name
### Rule 6.3 – Add the suffix _e to the Enum name
```c
typedef enum
{
…
}FullScale_e;
```
### Rule 6.4 – Enum elements name shall start with the prefix 
```c
<TypedefName_>
typedef enum
{
  FullScale_2g = 0,
  FullScale_4g,
  FullScale_8g,
  FullScale_16g,
}FullScale_e;
```
### Rule 6.5 – Use filename as a prefix to Enum name and hence the individual elements
```c
typedef enum
{
  Mpu6050_FullScale_2g = 0,
  Mpu6050_FullScale_4g,
  Mpu6050_FullScale_8g,
  Mpu6050_FullScale_16g,
}Mpu6050_FullScale_e;
```
## 7.	If-Condition
### Rule 7.1 – Avoid using single line condition
- Always use multiple line body and enclose with braces
if(accelZ > 800)
{
  printf("Mostly Level, Z-Accel = %d mg\n", accelZ);
}

### Rule 7.2 – When checking for multiple conditions, enclose conditions with parentheses
- Use Boolean conditional operators for multiple conditions relationship such as (OR: ||, AND: &&)
if((verticalSpeed > 12) && (horizontalSpeed < 5))
{
}

### Rule 7.3 – When comparing a variable with constant, put constant first on the left-hand side
- When you forget to put double equals =, compiler will detect an assignment to constant error when the constant is on the Left Hand Side, which can be very useful.
if(12 == verticalSpeed)
{
  
}

 
## 8.	Switch-Case
### Rule 8.1 – Use Enum as switch case variable, whenever possible
•	Avoid using generic types such as integer because they take on so many values.
Rule 8.2 – Do not put a default case when an Enum is used as the switch case variable
•	This allows compiler to catch missing or unhandled cases, and throw warning.
Mpu6050_AccelFullScale_e fullScaleSelect = Mpu6050_AccelFullScale_4g;
```c
switch(fullScaleSelect)
{
  case Mpu6050_AccelFullScale_2g:
    break;

  case Mpu6050_AccelFullScale_4g:
    break;

  case Mpu6050_AccelFullScale_8g:
    break;
  
  case Mpu6050_AccelFullScale_16g:
    break;
}
```
### Rule 8.3 – To define a new variable within a case, add a code block explicitly
•	Cases are considered labels in C and thus do not support variable definition by default.
•	Note: It is recommended to keep the break keyword inside the code block, for better readability.
 ```c
  case Mpu6050_FullScale_4g:
  {
    int value = 4;
    printf("FS %dg\n", value);
    break;
  }
```
### Rule 8.4 – If multiple cases have the same handling, use fall through.
•	Fall through is enabled by writing cases in series with just a single break at the end and a common body for all.
 ```c 
  case Mpu6050_FullScale_8g:
  case Mpu6050_FullScale_16g:
  case Mpu6050_FullScale_32g:
    printf("FS 8g or higher\n");
    break;
```
### Rule 8.5 – When using Non-Enum switch case variable, default case must be added
•	This is simply because integer variable can take on many different values, unlike Enum where elements are bounded.
int mySelect = 1;
```c
switch(mySelect)
{
  case 1:
    break;

  case 2:
    break;

  default:
    break;
}
```
 
## 9.	For-Loop
### Rule 9.1 – Avoid using magic numbers for loop counter
•	Use define constants instead.
•	When the constant is changed, all instances will be updated unlike magic numbers where you need to change them individually.
```c
#define ELEMENTS_COUNT  10

int main (void)
{
  for(uint8_t i = 0; i < ELEMENTS_COUNT; i++)
  {
    printf("Counter = %d\n", i);
  }
  getchar();
  return 0;
}
```
 
## 10.	Macros
Rule 10.1 – Use full upper-case letters for Macros
•	Separate words with underscore _
#define MAX_HEIGHT  100

### Rule 10.2 – When defining a series of defines, use double underscore __ to separate prefix text from individual elements name
#define MAIN_TASK_SIG__SUSPEND        os_SignalNum_0
#define MAIN_TASK_SIG__RESUME         os_SignalNum_1
#define MAIN_TASK_SIG__HEAP_MONITOR   os_SignalNum_2
#define MAIN_TASK_SIG__RUN_STATS      os_SignalNum_3

### Rule 10.3 – Enclose arithmetic operations within Macros with parentheses
#define MAX_HEIGHT  (50 * 2)

### Rule 10.4 – For parametric or function-like Macros, surround any use of Macro parameters with parentheses
- This will ensure that parameters arithmetic operations are resolved first
#define TEMPERATURE_CONVERT__C_TO_F(X)  (((X) * 1.8f) + 32.0f)

printf("46.30C = %.2fF\n", TEMPERATURE_CONVERT__C_TO_F(45.3 + 1.0f));

Macro Expansion:
---> (((45.3 + 1.0f) * 1.8f) + 32.0f)

---

## Conclusion
This guide provides structured rules to ensure consistency, readability, and maintainability in your C code. Follow the above guidelines to write clean and efficient C programs.
