# Accountant bot features

This document outlines the main features of the Accountant Bot, a sophisticated personal finance management tool.

The Accountant Bot is designed to be a comprehensive solution for tracking personal finances, incorporating popular features from existing apps and innovative ideas. While its primary function is expense tracking, it also allows users to record income, providing a complete picture of their financial health.

Key highlights:

- Expense and income tracking
- Multiple account management
- Customizable spending limits
- Detailed financial analytics

Technology stack:

- Language: Python version > 3.9
- Database: SQLite, SQLAlchemy
- Telegram bot framework: python-telegram-bot

Features:

1. Log in with telegram account: Different users may be able to use an app and account it's expenses.
2. A user can add income/outcome money
3. A user can add an amount of money that he has now in account.
4. When adding an expense, can add a comment
5. Expenses have a timestamp an may be grouped by day, week, month etc.
6. A user can set a limit up to which he can spend money daily.
7. A user can add multiple accounts, name them, set currency of an account, and set the amount of money in account. Example: Bank A, moneybox, Bank C, crypto ledger etc.
8. User can set an account as a default one and when adding an expense by default the money will be substracted from it, if not set different.
9. A user can request the current state of it's accounts (my current Net Worth).
10. A user can request a statement with analytics: for week, month with data about expenses and limits (if set)

## User stories

### **Must Have Features**

I apologize for overlooking the initial setup and onboarding process in the previous user stories. You're absolutely right—onboarding is crucial for guiding users through the initial setup, including creating their first account. Additionally, handling incomes like salary is an essential feature for a comprehensive personal finance tracker.

Let me update the user stories to include the onboarding process and income handling.

---

### **Must Have Features (Updated)**

#### **1. User Onboarding and Initial Account Setup**

---

**User Story:**

As a **new user**, I want to be **guided through an onboarding process** that helps me **set up my first account** and understand how to use the bot, so that I can **start tracking my expenses and incomes effectively**.

**Acceptance Criteria:**

- **Welcome Message:**
  - Upon first interaction, I receive a friendly welcome message introducing the bot's capabilities.
- **First Account Creation:**
  - I am prompted to create my first account.
  - I can input the account name, currency, and initial balance.
  - The account is saved and set as my default account.
- **Introduction to Core Features:**
  - The bot briefly explains how to add expenses and incomes.
  - Information is provided on how to change settings later.
- **Option to Set Daily Spending Limit:**
  - I am given the option to set a daily spending limit immediately or skip for later.
- **Confirmation:**
  - A summary of my initial setup is provided.
  - I am informed that I can start using the bot.

**Tasks:**

- **Welcome Message Implementation:**

  - Craft a welcoming message that introduces the bot and its main features.
  - Ensure the tone is friendly and encouraging.

- **First Account Prompt:**

  - Detect if the user is new (no existing data).
  - Prompt the user to create their first account:
    - Collect account name.
    - Collect currency (default to user's locale if possible).
    - Collect initial balance.
  - Validate inputs and handle any errors.

- **Set Default Account:**

  - Save the account information in the database linked to the user's Telegram ID.
  - Mark this account as the default account for transactions.

- **Introduce Core Features:**

  - Provide brief instructions on how to:
    - Add an expense.
    - Add income.
    - View balance.
  - Explain the use of inline keyboards and commands.

- **Option to Set Daily Spending Limit:**

  - Ask the user if they want to set a daily spending limit now.
  - If yes, prompt for the amount and save it in settings.
  - If no, inform them they can set it later in settings.

- **Confirmation Message:**

  - Summarize the user's initial setup:
    - Account name, currency, and balance.
    - Default account status.
    - Daily spending limit if set.
  - Encourage the user to start adding expenses and incomes.

- **Onboarding Flow Control:**
  - Allow the user to exit or skip onboarding steps if desired.
  - Ensure that all necessary data (like the first account) is collected before proceeding to regular use.

---

#### **2. Handling Incomes Like Salary**

---

**User Story:**

As a **user**, I want to **add income entries such as my salary**, so that I can **accurately track my financial inflows and see the impact on my account balances**.

**Acceptance Criteria:**

- **Income Entry Process:**

  - I can initiate adding income via a command or button (e.g., "Add Income").
  - The bot prompts me to enter the income amount.
  - Input validation ensures the amount is a positive number.

- **Bot Response:**

  - The bot responds with: "Add income: `[amount]` `[currency]` to `[default account]`."
  - An inline keyboard is displayed with options: "Choose Category," "Change Account," "Submit."

- **Choose Category Option:**

  - Upon selecting "Choose Category," I am presented with common income categories (e.g., Salary, Bonus, Gift).
  - An "Other" option allows me to input a custom category.
  - The income summary updates to include the chosen category.

- **Change Account Option:**

  - Upon selecting "Change Account," I can choose from my list of accounts.
  - The income summary updates with the selected account.

- **Submit Option:**

  - When I press "Submit," the income is recorded with the amount, account, category (if any), and timestamp.
  - My account balance updates accordingly.
  - I receive a confirmation message: "Income of `[amount]` `[currency]` to `[account]` in category `[category]` recorded successfully."

- **Income Inclusion in Reports:**
  - Incomes are reflected in daily, weekly, and monthly summaries.
  - Account balances and net worth calculations include income entries.

**Tasks:**

- **Add "Add Income" Functionality:**

  - Include an "Add Income" button in the main menu.
  - Enable income entry via a command like `/addincome`.

- **Implement Income Entry Flow:**

  - Prompt the user for the income amount upon selecting "Add Income."
  - Validate the input amount.

- **Develop Bot Response and Inline Keyboard:**

  - Create a response template for income similar to expense entry.
  - Display inline keyboard options: "Choose Category," "Change Account," "Submit."

- **Implement "Choose Category" for Income:**

  - Define a list of common income categories.
  - Allow for custom category input when "Other" is selected.
  - Update the income summary with the selected category.

- **Implement "Change Account" for Income:**

  - Fetch the user's accounts from the database.
  - Allow selection via inline keyboard.
  - Update the income summary with the selected account.

- **Handle "Submit" for Income:**

  - Save the income details in the database.
  - Update the balance of the selected account.
  - Send a confirmation message to the user.

- **Update Reporting Functions:**

  - Modify summaries and reports to include income data.
  - Ensure that net calculations accurately reflect incomes and expenses.

- **Error Handling:**
  - Validate all inputs and handle errors appropriately.
  - Ensure robust error messages and retry options.

---

#### **1. Expense Entry with Optional Details**

---

**User Story:**

As a **user**, I want to **quickly add an expense by entering the amount**, with the option to **choose a category or change the account**, so that I can **efficiently track my spending** with as much or as little detail as I prefer.

**Acceptance Criteria:**

- **Expense Amount Entry:**

  - When I enter a numerical amount, the bot recognizes it as an expense entry.
  - If I enter a non-numerical value or invalid number, the bot prompts me to re-enter a valid amount.

- **Bot Response:**

  - The bot replies with: "Add expense: `[amount]` `[currency]` from `[default account]`."
  - An inline keyboard is displayed with options: "Choose Category," "Change Account," and "Submit."

- **Choose Category Option:**

  - Upon selecting "Choose Category," I am presented with common expense categories via buttons.
  - There is an "Other" option allowing me to input a custom category.
  - After selection, the expense summary updates to include the chosen category.

- **Change Account Option:**

  - Upon selecting "Change Account," I can choose from my list of accounts.
  - After selection, the expense summary updates to include the selected account.

- **Submit Option:**
  - When I press "Submit," the expense is recorded with the amount, account, category (if any), and timestamp.
  - I receive a confirmation message: "Expense of `[amount]` `[currency]` from `[account]` in category `[category]` recorded successfully."

**Tasks:**

- **Implement Expense Amount Entry:**

  - Configure the bot to recognize numerical input as an expense amount.
  - Validate input to ensure it is a positive number.
  - Handle invalid inputs with appropriate error messages.

- **Develop Bot Response and Inline Keyboard:**

  - Create a response template including amount, currency, and default account.
  - Implement an inline keyboard with options: "Choose Category," "Change Account," "Submit."

- **Implement "Choose Category" Functionality:**

  - Define a list of common expense categories.
  - Create an inline keyboard to display categories and an "Other" option.
  - Allow custom category input when "Other" is selected.
  - Update the expense summary with the selected category.

- **Implement "Change Account" Functionality:**

  - Fetch the user's accounts from the database.
  - Display accounts via inline keyboard for selection.
  - Update the expense summary with the selected account.

- **Implement "Submit" Functionality:**
  - Save the expense details to the database.
  - Ensure timestamp and user association are recorded.
  - Send a confirmation message to the user.

---

#### **2. User Authentication via Telegram**

---

**User Story:**

As a **user**, I want the bot to **recognize me through my Telegram account**, so that I **don't need to create a separate login** and my data remains secure.

**Acceptance Criteria:**

- The bot automatically retrieves and stores my unique Telegram ID upon first interaction.
- All my data is associated with my Telegram ID.
- No additional login steps are required.
- My data is securely isolated from other users.

**Tasks:**

- **Implement User Identification:**

  - Configure the bot to capture the user's Telegram ID upon initial interaction.
  - Store the Telegram ID in the user table within the database.

- **Associate Data with User ID:**

  - Modify database schema to link accounts, expenses, and settings with the user's Telegram ID.
  - Ensure all bot actions use the Telegram ID to fetch and store user-specific data.

- **Ensure Data Isolation:**
  - Implement data access controls to prevent users from accessing others' data.
  - Test for security vulnerabilities related to user data access.

---

#### **3. Default Account Setup**

---

**User Story:**

As a **user**, I want to **have a default account set up**, so that I can **add expenses quickly without specifying the account each time**.

**Acceptance Criteria:**

- Upon first use, if no accounts exist, the bot prompts me to create a default account.
- I can input the account name and initial balance.
- The default account is automatically used in expense entries unless I choose to change it.
- I can change the default account in the settings menu.

**Tasks:**

- **Initial Account Prompt:**

  - After user authentication, check if the user has any accounts.
  - If none, prompt the user to create a default account.

- **Account Creation Process:**

  - Collect account name and initial balance from the user.
  - Validate inputs and handle errors accordingly.
  - Save the account details in the database linked to the user's Telegram ID.

- **Set Default Account:**

  - Mark the created account as the default in the user's settings.
  - Ensure the default account is used in expense entries unless changed.

- **Settings Menu Option:**
  - Add an option in the settings to change the default account.
  - Allow selection from existing accounts.

---

#### **4. Daily Spending Limit**

---

**User Story:**

As a **user**, I want to **set a daily spending limit**, so that I can **monitor my expenses and avoid overspending**.

**Acceptance Criteria:**

- I can set or update my daily spending limit in the settings.
- After each expense, the bot informs me of:
  - Total spent today.
  - Remaining amount before reaching the limit.
- I receive alerts when:
  - I reach 80% of my daily limit.
  - I exceed my daily limit.
- The daily total resets at the start of each day, based on my timezone.

**Tasks:**

- **Implement Limit Setting:**

  - Add a feature in settings for users to set or update their daily spending limit.
  - Validate the limit input to ensure it is a positive number.

- **Track Daily Expenses:**

  - Calculate the total expenses for the current day whenever a new expense is added.
  - Use timestamps to identify expenses made on the current day.

- **Provide Feedback After Expenses:**

  - Display the total spent today and remaining limit after each expense.
  - Format messages for clarity.

- **Implement Threshold Alerts:**

  - Set up checks to send alerts when the user reaches 80% and 100% of their daily limit.
  - Ensure alerts are sent promptly after the triggering expense is recorded.

- **Handle Daily Reset:**
  - Implement a mechanism to reset or calculate daily totals based on the user's timezone.
  - Consider daylight saving time adjustments if necessary.

---

#### **5. Basic Data Storage with SQLite**

---

**User Story:**

As a **developer**, I need to **store user data reliably**, so that the bot **functions correctly** and data **persists between sessions**.

**Acceptance Criteria:**

- The database stores:
  - User profiles linked by Telegram ID.
  - Account details and balances.
  - Expense records with all relevant details.
  - User settings, including default account and spending limit.
- Data operations (create, read, update, delete) function correctly.
- Data integrity is maintained, and relationships between data are properly established.

**Tasks:**

- **Design Database Schema:**

  - Create tables for users, accounts, expenses, and settings.
  - Define primary keys, foreign keys, and necessary indexes.
  - Establish relationships (e.g., a user has many accounts; an account has many expenses).

- **Implement ORM Models:**

  - Use SQLAlchemy or another ORM to interact with the SQLite database.
  - Define models corresponding to each table.
  - Implement methods for CRUD operations.

- **Data Access Layer:**

  - Write functions or classes to handle database interactions.
  - Ensure proper handling of database sessions and connections.

- **Data Validation and Integrity:**
  - Implement constraints (e.g., non-null fields, data types).
  - Handle exceptions and errors related to database operations.

---

#### **6. Error Handling**

---

**User Story:**

As a **user**, I want to **receive helpful feedback when I make an input error**, so that I can **correct it easily** and continue using the bot smoothly.

**Acceptance Criteria:**

- When I input invalid data, the bot informs me with a clear and concise error message.
- Error messages specify what went wrong and how to fix it.
- I can retry the action after an error without starting over.
- The bot handles unexpected errors gracefully, providing a generic error message and logging the issue internally.

**Tasks:**

- **Input Validation:**

  - Validate all user inputs, including amounts, account names, and settings.
  - Check for correct data types and acceptable ranges/values.

- **Error Messages:**

  - Create user-friendly error messages for common issues.
    - Example: "Invalid amount entered. Please enter a positive number."

- **Retry Mechanism:**

  - After an error, prompt the user to re-enter the information.
  - Maintain the context of the current operation.

- **Exception Handling:**

  - Implement try-except blocks around code that may raise exceptions.
  - Handle known exceptions with specific actions.
  - Log unexpected errors for debugging purposes.

- **User Notification for System Errors:**
  - If a critical error occurs, inform the user with a generic message.
    - Example: "An error occurred while processing your request. Please try again later."

---

### **Should Have Features**

#### **7. Inline Keyboard Interface**

---

**User Story:**

As a **user**, I want to **use buttons within the chat**, so that I can **easily navigate and perform actions without typing commands**.

**Acceptance Criteria:**

- Inline keyboards are used for primary interactions, such as:
  - Expense options ("Choose Category," "Change Account," "Submit").
  - Menu navigation.
  - Settings adjustments.
- Buttons are clearly labeled and intuitive.
- The bot responds appropriately to button selections.

**Tasks:**

- **Integrate Inline Keyboards:**

  - Implement inline keyboards for expense entry and menus.
  - Ensure buttons trigger the correct callbacks and functions.

- **User Interface Design:**

  - Design button labels for clarity and brevity.
  - Arrange buttons logically for ease of use.

- **Handle Button Callbacks:**

  - Write handlers for each button action.
  - Maintain the state of the conversation context.

- **Testing:**
  - Test the inline keyboards across different devices and platforms.
  - Ensure accessibility and responsiveness.

---

#### **8. Undo Last Transaction**

---

**User Story:**

As a **user**, I want to **undo my last expense entry**, so that I can **correct mistakes immediately**.

**Acceptance Criteria:**

- I can initiate an undo action via a command or button.
- The bot confirms the action before proceeding.
- Upon confirmation, the last expense is removed from records.
- The bot confirms the successful undo operation.
- Expense summaries and limits are updated accordingly.

**Tasks:**

- **Implement Undo Command/Button:**

  - Add an "Undo Last Expense" option in the main menu or via a command like `/undo`.

- **Retrieve Last Expense:**

  - Fetch the most recent expense entry for the user from the database.

- **Confirmation Prompt:**

  - Ask the user to confirm the undo action.
    - Example: "Are you sure you want to undo the last expense of `[amount]` `[currency]`?"

- **Process Undo Action:**

  - If confirmed, delete the expense record from the database.
  - Update daily totals and any affected calculations.

- **Provide Feedback:**
  - Inform the user that the expense has been successfully undone.
  - Display the updated daily total and remaining limit.

---

#### **9. Basic Reporting**

---

**User Story:**

As a **user**, I want to **view summaries of my spending**, so that I can **understand my financial habits over time**.

**Acceptance Criteria:**

- I can request summaries for different periods:
  - Daily
  - Weekly
  - Monthly
- The bot provides:
  - Total expenses for the period.
  - Breakdown by category (if applicable).
  - Comparison against spending limits.
- The report is formatted for easy readability.

**Tasks:**

- **Implement Summary Commands/Buttons:**

  - Create commands like `/daily_summary`, `/weekly_summary`, `/monthly_summary`.
  - Alternatively, add options in the main menu.

- **Generate Reports:**

  - Query the database for expenses within the specified period.
  - Calculate totals and categorize expenses.

- **Format Reports:**

  - Organize information clearly with headings and bullet points.
  - Use markdown formatting for emphasis (e.g., bold totals).

- **Handle Edge Cases:**
  - If no data is available for the period, inform the user appropriately.
    - Example: "No expenses recorded for this period."

---

#### **10. Settings Menu**

---

**User Story:**

As a **user**, I want to **access and modify my settings**, so that I can **customize the bot to my preferences**.

**Acceptance Criteria:**

- I can access settings via a command or menu option.
- Available settings include:
  - Changing the default account.
  - Updating the daily spending limit.
  - Managing categories (if implemented).
- Navigation within the settings menu is intuitive.
- Changes are saved and applied immediately.

**Tasks:**

- **Create Settings Command/Menu:**

  - Implement a `/settings` command or a "Settings" button in the main menu.

- **Develop Settings Options:**

  - For each setting, provide an interface for viewing and modifying the value.
  - Use inline keyboards where appropriate.

- **Implement Setting Modifications:**

  - Handle input and validation for each setting change.
  - Update the user's settings in the database.

- **Ensure User Feedback:**

  - Confirm changes to the user.
    - Example: "Your daily spending limit has been updated to `[amount]` `[currency]`."

- **Navigation:**
  - Allow users to return to the main menu from the settings.
  - Provide options to navigate back to previous settings screens.

---

### **Notes for the Developer**

- **Focus on Must Have Features First:**

  - Prioritize implementing the core functionalities required for the MVP.
  - Ensure each feature is thoroughly tested before moving on.

- **Maintain Code Quality:**

  - Follow best practices for coding, including clear naming conventions and documentation.
  - Write modular code to facilitate future enhancements.

- **User Experience Considerations:**

  - Keep interactions concise to prevent overwhelming the user.
  - Provide helpful guidance during the initial use to familiarize users with the bot's functions.

- **Data Security:**

  - Protect user data by implementing appropriate security measures.
  - Be mindful of privacy considerations when handling personal and financial information.

- **Testing:**
  - Test features incrementally as they are developed.
  - Perform integration testing to ensure all components work seamlessly together.

---
