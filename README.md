<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Education Platform</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f9;
            color: #333;
            line-height: 1.6;
            transition: background-color 0.3s, color 0.3s;
        }

        header {
            background-color: #6a1b9a;
            color: white;
            padding: 20px 30px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
        }

        header h1 {
            margin: 0;
            font-size: 28px;
            font-weight: 300;
        }

        .cart {
            font-size: 18px;
        }

        .theme-switch {
            margin-left: 20px;
            cursor: pointer;
        }

        .video-list {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            margin: 20px;
        }

        .video-item {
            border: 2px solid #6a1b9a;
            background-color: white;
            border-radius: 10px;
            margin: 15px;
            padding: 15px;
            width: 260px;
            box-sizing: border-box;
            transition: transform 0.3s, box-shadow 0.3s;
            text-align: center;
        }

        .video-item:hover {
            transform: scale(1.05);
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
        }

        .modal {
            display: none;
            position: fixed;
            z-index: 1;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0, 0, 0, 0.6);
        }

        .modal-content {
            background-color: #ffffff;
            margin: 10% auto;
            padding: 30px;
            border-radius: 10px;
            width: 90%;
            max-width: 400px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
        }

        .close {
            color: #aaa;
            float: right;
            font-size: 28px;
            font-weight: bold;
        }

        .close:hover,
        .close:focus {
            color: #000;
            text-decoration: none;
            cursor: pointer;
        }

        #videoPlayer {
            display: none;
            margin: 20px auto;
            text-align: center;
        }

        video {
            width: 80%;
            height: auto;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
        }

        .payment-form {
            display: flex;
            flex-direction: column;
        }

        .payment-form label {
            margin-top: 10px;
            font-weight: bold;
        }

        .payment-form input {
            padding: 10px;
            margin-top: 5px;
            border: 2px solid #6a1b9a;
            border-radius: 5px;
            transition: border-color 0.3s;
        }

        .payment-form input:focus {
            border-color: #d5006d;
            outline: none;
        }

        .payment-form button {
            margin-top: 20px;
            padding: 12px;
            background-color: #d5006d;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
            font-size: 16px;
        }

        .payment-form button:hover {
            background-color: #b0003a;
        }

        .alert {
            color: red;
            font-weight: bold;
            margin-top: 10px;
        }

        /* Dark mode styles */
        body.dark-mode {
            background-color: #333;
            color: #f4f4f9;
        }

        header.dark-mode {
            background-color: #444;
        }

        .video-item.dark-mode {
            background-color: #555;
            border-color: #d5006d;
        }

        .modal-content.dark-mode {
            background-color: #444;
        }

        .payment-form input.dark-mode {
            border: 2px solid #d5006d;
            background-color: #555;
            color: white;
        }

        .payment-form button.dark-mode {
            background-color: #d5006d;
        }

        .payment-form button.dark-mode:hover {
            background-color: #b0003a;
        }

        .theme-switch span {
            color: white;
            font-size: 18px;
        }

    </style>
</head>
<body>
    <header>
        <h1>Education Platform</h1>
        <div class="cart">
            <button id="cartButton">Cart (<span id="cartCount">0</span>)</button>
        </div>
        <div class="theme-switch">
            <span id="themeToggle">🌙</span>
        </div>
    </header>
    <main>
        <h2 style="text-align: center;">Available Videos</h2>
        <div id="videoList" class="video-list">
            <!-- Video items will be generated here -->
        </div>
        <div id="videoPlayer">
            <h2>Now Playing:</h2>
            <video id="currentVideo" controls>
                <source id="videoSource" src="" type="video/mp4">
                Your browser does not support the video tag.
            </video>
        </div>
    </main>
    <div id="cartModal" class="modal">
        <div class="modal-content">
            <span class="close">&times;</span>
            <h2>Your Cart</h2>
            <div id="cartItems"></div>
            <button id="checkoutButton">Checkout</button>
        </div>
    </div>
    <div id="paymentModal" class="modal">
        <div class="modal-content">
            <span class="close">&times;</span>
            <h2>Payment</h2>
            <div class="payment-form">
                <label for="cardName">Name on Card:</label>
                <input type="text" id="cardName" required>

                <label for="cardNumber">Card Number:</label>
                <input type="text" id="cardNumber" maxlength="16" required oninput="this.value = this.value.replace(/[^0-9]/g, '');">


                <label for="expirationDate">Expiration Date (MM/YY):</label>
                <input type="text" id="expirationDate" required>

                <label for="cvv">CVV:</label>
                <input type="text" id="cvv" maxlength="3" required oninput="this.value = this.value.replace(/[^0-9]/g, '');">


                <label for="billingAddress">Billing Address:</label>
                <input type="text" id="billingAddress" value="" required>

                <label for="country">Country:</label>
                <select id="country" required>
                   <option value="United States">United States</option>
                   <option value="Canada">Canada</option>
                   <option value="United Kingdom">United Kingdom</option>
                   <option value="Australia">Australia</option>
                   <option value="India">India</option>
                   <option value="Germany">Germany</option>
                   <option value="France">France</option>
                   <option value="China">China</option>
                   <option value="Japan">Japan</option>
                   <option value="Brazil">Brazil</option>
</select>


                <label for="postalCode">Postal Code:</label>
                <input type="text" id="postalCode" required>

                <p>Total: $<span id="totalAmount">0</span></p>
                <button id="payButton">Pay</button>
            </div>
        </div>
    </div>
    <script>
        const videos = [
            { id: 1, title: 'HTML Basics', price: 10, url: 'https://www.w3schools.com/html/mov_bbb.mp4' },
            { id: 2, title: 'CSS Fundamentals', price: 15, url: 'https://www.w3schools.com/html/mov_bbb.mp4' },
            { id: 3, title: 'JavaScript for Beginners', price: 20, url: 'https://www.w3schools.com/html/mov_bbb.mp4' },
            { id: 4, title: 'React Overview', price: 25, url: 'https://www.w3schools.com/html/mov_bbb.mp4' }
        ];

        let cart = [];

        // Function to display videos
        function displayVideos() {
            const videoList = document.getElementById('videoList');
            videos.forEach(video => {
                const videoItem = document.createElement('div');
                videoItem.className = 'video-item';
                videoItem.innerHTML = `
                    <h3>${video.title}</h3>
                    <p>Price: $${video.price}</p>
                    <button onclick="addToCart(${video.id})">Add to Cart</button>
                `;
                videoList.appendChild(videoItem);
            });
        }

        // Add video to cart
        function addToCart(id) {
            const video = videos.find(v => v.id === id);
            if (video) {
                cart.push(video);
                document.getElementById('cartCount').innerText = cart.length;
                alert(`${video.title} has been added to your cart!`);
            }
        }

        // Display cart
        function displayCart() {
            const cartItems = document.getElementById('cartItems');
            cartItems.innerHTML = ''; // Clear previous items
            cart.forEach((item, index) => {
                cartItems.innerHTML += `<p>${item.title} - $${item.price}</p>`;
            });
        }

        const cartButton = document.getElementById('cartButton');
        const cartModal = document.getElementById('cartModal');
        const closeCartButton = cartModal.getElementsByClassName('close')[0];

        cartButton.onclick = function() {
            displayCart();
            cartModal.style.display = 'block';
        }

        closeCartButton.onclick = function() {
            cartModal.style.display = 'none';
        }

        // Checkout button
        const checkoutButton = document.getElementById('checkoutButton');

        checkoutButton.onclick = function() {
            if (cart.length > 0) {
                const totalAmount = cart.reduce((total, item) => total + item.price, 0);
                document.getElementById('totalAmount').innerText = totalAmount;
                cartModal.style.display = 'none'; // Hide cart modal
                paymentModal.style.display = 'block'; // Show payment modal
            } else {
                alert('Your cart is empty!');
            }
        }

        // Modal functionality for payment
        const payButton = document.getElementById('payButton');

        payButton.onclick = function() {
            // Get card information (In a real application, validate this info)
            const cardName = document.getElementById('cardName').value;
            const cardNumber = document.getElementById('cardNumber').value;
            const expirationDate = document.getElementById('expirationDate').value;
            const cvv = document.getElementById('cvv').value;
            const billingAddress = document.getElementById('billingAddress').value;
            const country = document.getElementById('country').value;
            const postalCode = document.getElementById('postalCode').value;

            // Simulate successful payment
            if (cardName && cardNumber && expirationDate && cvv && billingAddress && country && postalCode) {
                alert('Payment Successful! Enjoy your video!');
                const videoToPlay = cart[0]; // Play the first video in the cart
                const videoSource = document.getElementById('videoSource');
                videoSource.src = videoToPlay.url;
                document.getElementById('currentVideo').load(); // Load the video
                document.getElementById('videoPlayer').style.display = 'block'; // Show the video player
                paymentModal.style.display = 'none'; // Hide payment modal
                cart = []; // Clear cart
                document.getElementById('cartCount').innerText = cart.length;
            } else {
                alert('Please fill in all fields!');
            }
        }

        // Theme toggle functionality
        const themeToggle = document.getElementById('themeToggle');
        themeToggle.onclick = function() {
            document.body.classList.toggle('dark-mode');
            document.querySelector('header').classList.toggle('dark-mode');
            const videoItems = document.querySelectorAll('.video-item');
            videoItems.forEach(item => item.classList.toggle('dark-mode'));
            const modalContents = document.querySelectorAll('.modal-content');
            modalContents.forEach(content => content.classList.toggle('dark-mode'));
            const paymentInputs = document.querySelectorAll('.payment-form input');
            paymentInputs.forEach(input => input.classList.toggle('dark-mode'));
            const paymentButton = document.querySelector('.payment-form button');
            paymentButton.classList.toggle('dark-mode');
            themeToggle.innerText = document.body.classList.contains('dark-mode') ? '🌞' : '🌙'; // Change icon
        }

        // Initial video display
        displayVideos();
    </script>
