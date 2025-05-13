<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Daksh Medical Solutions</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 20px;
      background-color: #f4f4f4;
    }
    header {
      text-align: center;
      background-color: #28A745;
      color: white;
      padding: 10px;
    }
    h2 {
      color: #333;
    }
    #product-list {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 20px;
    }
    .product-card {
      background-color: white;
      padding: 10px;
      border: 1px solid #ddd;
      text-align: center;
    }
    form {
      margin-top: 20px;
      background-color: white;
      padding: 20px;
      border: 1px solid #ddd;
    }
    input, select, button {
      margin: 10px 0;
      padding: 10px;
      width: 100%;
      box-sizing: border-box;
    }
    button {
      background-color: #28A745;
      color: white;
      border: none;
      cursor: pointer;
    }
    button:hover {
      background-color: #218838;
    }
    #order-confirmation {
      margin-top: 20px;
      color: green;
    }
    #main-content {
      display: block;
    }
    #offline {
      display: none;
      max-width: 90%;
      padding: 20px;
      background-color: #fff;
      border: 1px solid #ddd;
      text-align: center;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <header>
    <h1>Daksh Medical Solutions</h1>
  </header>
  <main id="main-content">
    <h2>Products</h2>
    <div id="product-list">
      <div class="product-card">
        <h3>Sample Equipment 1</h3>
        <p>Price: ₹1000</p>
        <p>Sample Medical Device</p>
      </div>
      <div class="product-card">
        <h3>Sample Equipment 2</h3>
        <p>Price: ₹2000</p>
        <p>Sample Medical Device</p>
      </div>
    </div>
    <h2>Place Order</h2>
    <form id="order-form">
      <input type="text" id="client-name" placeholder="Your Name" required><br>
      <input type="tel" id="client-phone" placeholder="Phone Number" required><br>
      <input type="text" id="client-address" placeholder="Address" required><br>
      <select id="product-select" required>
        <option value="">Select Product</option>
        <option value="Sample Equipment 1">Sample Equipment 1</option>
        <option value="Sample Equipment 2">Sample Equipment 2</option>
      </select><br>
      <button type="submit">Submit Order</button>
    </form>
    <div id="order-confirmation"></div>
  </main>
  <div id="offline">
    <h2>No Internet Connection</h2>
    <p>Please connect to the internet to place orders or view products.</p>
    <button onclick="checkConnection()">Retry</button>
  </div>

  <script>
    const mainContent = document.getElementById("main-content");
    const offlineDiv = document.getElementById("offline");
    const orderForm = document.getElementById("order-form");
    const orderConfirmation = document.getElementById("order-confirmation");

    function checkConnection() {
      try {
        if (navigator.onLine) {
          mainContent.style.display = "block";
          offlineDiv.style.display = "none";
          requestNotificationPermission();
        } else {
          mainContent.style.display = "none";
          offlineDiv.style.display = "block";
        }
      } catch (error) {
        mainContent.style.display = "block";
        offlineDiv.style.display = "none";
      }
    }

    function requestNotificationPermission() {
      try {
        if ("Notification" in window) {
          Notification.requestPermission().then(permission => {
            if (permission === "granted") {
              new Notification("Daksh Medical Solutions", {
                body: "You are online! Check our latest products and offers.",
                icon: "https://via.placeholder.com/128/28A745/000?text=Logo"
              });
            }
          });
        }
      } catch (error) {
        console.log("Notification error:", error);
      }
    }

    orderForm.addEventListener("submit", (e) => {
      e.preventDefault();
      const name = document.getElementById("client-name").value;
      const phone = document.getElementById("client-phone").value;
      const address = document.getElementById("client-address").value;
      const product = document.getElementById("product-select").value;
      
      if (name && phone && address && product) {
        orderConfirmation.textContent = `Order placed! ${name}, we will contact you at ${phone} for ${product} delivery at ${address}.`;
        orderForm.reset();
        if (navigator.onLine) {
          requestNotificationPermission();
        }
      } else {
        orderConfirmation.textContent = "Please fill all fields!";
      }
    });

    // Initial check
    try {
      checkConnection();
      window.addEventListener("online", checkConnection);
      window.addEventListener("offline", checkConnection);
      setInterval(() => {
        if (offlineDiv.style.display === "block") {
          checkConnection();
        }
      }, 10000);
    } catch (error) {
      mainContent.style.display = "block";
      offlineDiv.style.display = "none";
    }
  </script>
</body>
</html>
