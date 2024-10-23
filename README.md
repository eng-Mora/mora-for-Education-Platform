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
    </style>
</head>
<body>
    <header>
        <h1>Education Platform</h1>
        <div class="cart">
            <button id="cartButton">Cart (<span id="cartCount">0</span>)</button>
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
                <input type="text" id="cardNumber" required>

                <label for="expirationDate">Expiration Date (MM/YY):</label>
                <input type="text" id="expirationDate" required>

                <label for="cvv">CVV:</label>
                <input type="text" id="cvv" required>

                <label for="billingAddress">Billing Address:</label>
                <input type="text" id="billingAddress" value="123 Market St." required>

                <label for="country">Country:</label>
                <input type="text" id="country" value="United States" required>

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

        // Function to add video to cart
        function addToCart(videoId) {
            const video = videos.find(v => v.id === videoId);
            cart.push(video);
            document.getElementById('cartCount').innerText = cart.length;
            alert(`${video.title} added to cart!`);
        }

        // Function to display cart
        function displayCart() {
            const cartItems = document.getElementById('cartItems');
            cartItems.innerHTML = '';
            cart.forEach((item, index) => {
                cartItems.innerHTML += `<p>${item.title} - $${item.price} <button onclick="removeFromCart(${index})">Remove</button></p>`;
            });
        }

        // Function to remove video from cart
        function removeFromCart(index) {
            cart.splice(index, 1);
            document.getElementById('cartCount').innerText = cart.length;
            displayCart();
        }

        // Modal functionality for cart
        const cartModal = document.getElementById('cartModal');
        const cartButton = document.getElementById('cartButton');
        const closeCartButton = document.getElementsByClassName('close')[0];

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

        // Initial video display
        displayVideos();
    </script>
