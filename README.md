# employee-management-system-in-react-js

# Employee Management System

This is a fully functional **Employee Management System** built with **React** and **Context API**, featuring role-based access for **Admin** and **Employees**. It demonstrates task assignment, task tracking, and workflow management in a company.

---

## 🔹 Demo Credentials

### Admin Panel

| Email                                         | Password |
| --------------------------------------------- | -------- |
| [admin@example.com](mailto:admin@example.com) | 123      |

### Employee Panel

| Email                           | Password |
| ------------------------------- | -------- |
| [emp1@e.com](mailto:emp1@e.com) | 123      |
| [emp2@e.com](mailto:emp2@e.com) | 123      |
| [emp3@e.com](mailto:emp3@e.com) | 123      |
| [emp4@e.com](mailto:emp4@e.com) | 123      |
| [emp5@e.com](mailto:emp5@e.com) | 123      |

---

## 🔹 Features

### Admin Dashboard

- **Overview:** See total employees, total tasks for today, and task status summary.
- **Employees Section:**

  - List of all employees with **Name, Email, Salary**.
  - Click **View** to see detailed employee information including tasks.

- **Create Task Section:**

  - Assign tasks to specific employees through a simple form.

### Employee Dashboard

- **Overview:** View your personal information and task summary.
- **Task Status:**

  - **Pending** – tasks assigned but not completed.
  - **In Review** – tasks submitted for admin approval.
  - **Completed** – tasks approved by admin.
  - **Failed** – tasks rejected by admin.

- **Resubmission:** Employees can resubmit failed tasks for re-evaluation.

---

## 🔹 Technology Stack

- **Frontend:** React.js, Tailwind CSS
- **State Management:** React Context API
- **Form Validation:** Built-in HTML validation and state checks

---

## 🔹 Workflow Overview

1. **Admin** logs in → views dashboard → manages employees → assigns tasks.
2. **Employee** logs in → views own tasks → completes and submits tasks → admin approves or rejects.
3. Tasks flow through the statuses: **Pending → In Review → Approved / Rejected**.

<!-- --- -->

<!-- ## 🔹 Screenshots (Optional)


---

## 🔹 Getting Started

1. Clone the repository:

   ```bash
   git clone https://github.com/Abhishek-mehata/ems.git
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Start the project:

   ```bash
   npm start
   ```

4. Open [http://localhost:5173/](http://localhost:5173/) in your browser.

---

## 🔹 License

MIT License – feel free to use and modify.
