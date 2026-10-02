# NamasteMart — Full-Stack Java E-Commerce Platform 🛒

A **production-style e-commerce web application** built with **Java Servlets, JSP, JDBC, MySQL, Maven, JavaMail, and Google Gemini**. Features user registration, product catalog, cart, order placement, shipment tracking, admin dashboard with charts, demand/waitlist, transactional emails, and an AI chatbot.


## 📸 Preview
<img width="1913" height="960" alt="image" src="https://github.com/user-attachments/assets/de58f6bd-ed6f-470f-9d51-7e106d4ca6e8" />
<img width="1917" height="975" alt="image" src="https://github.com/user-attachments/assets/04049b67-397e-4c1d-a195-4b7c1b72b1e5" />
<img width="1906" height="962" alt="image" src="https://github.com/user-attachments/assets/4d06e14e-04df-4997-bffc-c6a86271a35b" />
<img width="1898" height="965" alt="image" src="https://github.com/user-attachments/assets/0e7d3d28-fc06-43e9-9447-804bb18faac6" />


## 📖 Overview

**NamasteMart** is a complete e-commerce web application built with the classic **Java EE stack (Servlets + JSP + JDBC)**. It demonstrates the full product-to-cash workflow:

1. **User** registers, logs in, browses products, adds to cart
2. **System** tracks out-of-stock items as **demands** and emails users when the product is available
3. **User** places an order → **transaction** is recorded, stock is decremented
4. **Admin** views orders, ships items, manages products and users, and views monthly sales/profit analytics
5. **Chatbot** answers customer questions via **Gemini API** integration

**Built to practice:**
- **Servlets + JSP** — clean MVC: Servlets as controllers, JSP as views, Beans as models
- **JDBC with MySQL** — PreparedStatements, transactions, BLOB image handling, foreign-key-aware schema
- **Service layer pattern** — `*Service` interfaces + `*ServiceImpl` implementations
- **JavaMail (Jakarta Mail)** — transactional emails for registration, order, shipping, cancellation, demand
- **Multipart file upload** — product & profile images stored as MySQL BLOBs
- **Externalised config** — `application.properties` for DB and mailer credentials
- **Maven build** — proper WAR packaging with dependency management
- **REST integration** — Gemini Chatbot via `HttpURLConnection`


## ✨ Features

### Customer
- 📝 **Register with profile image** (multipart upload)
- 🔐 **Login / logout** with session management
- 🔎 **Search** and filter products by type (15 categories)
- 🛒 **Cart** — add, update quantity, remove
- 📦 **Place order** → transaction recorded, mail sent
- 📜 **Order history** with shipped/pending status
- ❌ **Cancel order** with mock refund flow + mail
- 💌 **Demand list** — get emailed when out-of-stock items return
- 🤖 **AI chatbot** — Gemini-powered support widget
- 👤 **Profile management** with image upload

### Admin
- ➕ **Add / update / remove products** (with images)
- 📊 **Dashboard** — total products, orders, amount, monthly sales/profit (Chart.js)
- 🚚 **Ship orders** with mail notification
- 👥 **User management** — activate, deactivate, delete
- 📥 **Demand analytics** — see which products users are waiting for
- 💰 **Transactions** — view all, trigger refunds


## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Language | Java (JDK 8+) |
| Web | Servlets 3.1, JSP, JSTL |
| Database | MySQL 8 via JDBC |
| Build | **Maven** (WAR packaging) |
| Mail | Jakarta Mail 2.0 |
| AI | Google Gemini 1.5 Flash API |
| JSON | Gson 2.10 |
| UI | Bootstrap 3.4, Font Awesome, Chart.js |
| Server | Apache Tomcat 9+ |


## 📂 Project Structure

```
namaste-mart/
│
├── src/
│   └── com/namastemart/
│       ├── beans/              → POJOs (UserBean, ProductBean, CartBean, ...)
│       ├── constants/          → IUserConstants (column names)
│       ├── service/            → Interfaces (UserService, ProductService, ...)
│       │   └── impl/           → JDBC implementations
│       ├── srv/                → HttpServlets (LoginSrv, RegisterSrv, AddtoCart, ...)
│       └── utility/            → DBUtil, MailMessage, JavaMailUtil, IDUtil
│
├── WebContent/
│   ├── WEB-INF/
│   │   ├── web.xml
│   │   └── application.properties.example
│   ├── css/changes.css
│   ├── images/
│   ├── *.jsp                   → 20+ JSP views
│   ├── header.jsp, footer.html, adminheader.jsp, profileHeader.jsp
│   └── chatBot.html
│
├── DB_Query.sql                → schema + sample data
├── pom.xml
├── .gitignore
└── README.md
```


## 🗄️ Database Schema

7 tables with foreign keys:

```
user              → users (email PK, name, mobile, address, pincode, password, image)
product           → products (pid PK, name, type, info, price, quantity, image)
orders            → order line items (orderid + prodid PK, qty, amount, shipped, order_date)
transactions      → payment (transid PK, username FK, time, amount, status)
usercart          → cart items (username FK, prodid FK, qty)
user_demand       → waitlist (username FK, prodid FK, qty)
```

Run `DB_Query.sql` to create the schema and seed sample products.


## ⚙️ Setup & Deployment

### Prerequisites
- **JDK 8+**
- **MySQL 8+**
- **Maven 3.6+**
- **Apache Tomcat 9+** (or any Servlet 3.1 container)

### Step 1 — Clone
```bash
git clone https://github.com/YashdeepKaur28/namaste-mart.git
cd namaste-mart
```

### Step 2 — Create the database
```bash
mysql -u root -p < DB_Query.sql
```

### Step 3 — Configure credentials
```bash
cp WebContent/WEB-INF/application.properties.example \
   WebContent/WEB-INF/classes/application.properties
```

Edit `application.properties` with your:
- MySQL credentials (`db.username`, `db.password`)
- Gmail address + **app password** (`mailer.email`, `mailer.password`)
- Gemini API key (`chatbot.apiKey`)

⚠️ **Never commit the real `application.properties`** — it's gitignored.

### Step 4 — Build the WAR
```bash
mvn clean package
```

Produces `target/NamasteMart-0.0.1-SNAPSHOT.war`.

### Step 5 — Deploy
Drop the WAR into Tomcat's `webapps/` folder and start Tomcat.

### Step 6 — Access
```
http://localhost:8080/NamasteMart/
```

### Default admin login
Set the admin email/password in `application.properties` (via `admin.email` and `admin.password`). 🔴 **Change before deploying to production.**


## 🧠 Architecture

```
com.namastemart
├── beans/          → POJOs
├── constants/      → IUserConstants
├── service/        → Interfaces
│   └── impl/       → JDBC implementations
├── srv/            → HttpServlets
└── utility/        → DBUtil, MailMessage, JavaMailUtil, IDUtil
```

**Flow:**
1. JSP form → **Servlet** (`srv/`)
2. Servlet → **Service interface** (`service/`)
3. Service → **ServiceImpl** (`service/impl/`) — does JDBC work
4. ServiceImpl → **DBUtil** → MySQL
5. Servlet forwards to next **JSP** with a status message


## 🔗 Servlet Mappings

| URL | Servlet | Purpose |
|-----|---------|---------|
| `/LoginSrv` | `LoginSrv` | Login (admin + customer) |
| `/RegisterSrv` | `RegisterSrv` | New user registration |
| `/LogoutSrv` | `LogoutSrv` | Invalidate session |
| `/AddtoCart` | `AddtoCart` | Add/remove from cart |
| `/UpdateToCart` | `UpdateToCart` | Update cart quantity |
| `/AddProductSrv` | `AddProductSrv` | Admin: add product |
| `/RemoveProductSrv` | `RemoveProductSrv` | Admin: remove product |
| `/UpdateProductSrv` | `UpdateProductSrv` | Admin: update product |
| `/OrderServlet` | `OrderServlet` | Place order |
| `/ShipmentServlet` | `ShipmentServlet` | Admin: ship order |
| `/ImageServlet` | `ImageServlet` | Serve user profile images |
| `/ShowImage` | `ShowImage` | Serve product images |
| `/UpdateProfileServlet` | `UpdateProfileServlet` | Update user profile |
| `/fansMessage` | `FansMessage` | Contact form email |
| `/chatbot` | `ChatbotServlet` | Gemini API proxy |


## 🧠 Key Concepts Demonstrated

- **MVC with Servlets + JSP + Beans** — clean separation
- **Service layer pattern** — interfaces for testability, impls for JDBC
- **JDBC PreparedStatements** — SQL injection safe
- **BLOB handling** — product & profile images stored as `LONGBLOB`
- **Multipart uploads** — `@MultipartConfig` + `Part.getInputStream()`
- **Foreign-key-aware schema** — cascading deletes, referential integrity
- **Transactional emails** — registration, order, ship, cancel, demand-available
- **Session-based auth** — `HttpSession` + role checks
- **Externalized config** — `application.properties` via `ResourceBundle`
- **Maven WAR build** — `pom.xml` with dependencies
- **REST integration** — Gemini chatbot via `HttpURLConnection`
- **Aggregate analytics** — monthly sales/profit via SQL `GROUP BY` + Chart.js
- **Demand/waitlist pattern** — track out-of-stock interest, email when restocked
- **Order state machine** — `shipped` field drives order status (0=placed, 1=shipped, 2=cancelled)


## 🚀 Future Enhancements

- [ ] Migrate to **Spring Boot + Spring Data JPA** (biggest modernization)
- [ ] Replace JSPs with **Thymeleaf** or a React SPA
- [ ] **Hash passwords** with BCrypt — currently plaintext
- [ ] **Real payment gateway** — integrate Razorpay/Stripe instead of mock refunds
- [ ] **Pagination** for product listing
- [ ] **Search with filters** — price range, category, rating
- [ ] **Product reviews and ratings**
- [ ] **Wishlist**
- [ ] **Order tracking** with timestamps per state
- [ ] **REST API** for a mobile client
- [ ] **Unit tests** for service layer (JUnit + Mockito)
- [ ] **Docker** + `docker-compose` for one-command startup
- [ ] Move images to **S3** or similar object storage

## ⚠️ Known Limitations

- **Passwords stored plaintext** — needs BCrypt
- **Admin credentials in config** — should live in the DB
- **Images as LONGBLOB** — consider object storage at scale
- **No pagination** — large product catalogs will slow down
- **Mock refund** — no real payment gateway
- **No CSRF protection** — add tokens to state-changing forms
- **Business logic in JSPs** — some pages mix view + control (legacy pattern)


## 👩‍💻 Author

**Yashdeep Kaur**
- 🎓 B.Tech CSE, Punjabi University, Patiala (2026)
- 💼 Java Full Stack Trainee @ CodeSquadz
- 📧 ykdeep2453@gmail.com
- 🔗 [LinkedIn](https://linkedin.com/in/yashdeep-kaur-16aa083b1)
- 🐙 [@YashdeepKaur28](https://github.com/YashdeepKaur28)


⭐ If you found this useful, consider giving it a star!
