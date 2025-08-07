<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>G Marketplace</title>
    <!-- Use Tailwind CSS for a modern, responsive design -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Material Symbols for the icon -->
    <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@24,400,0,0" />
    <style>
        :root {
            --banner-height: 48px; /* Approximate height of the sticky banner */
            --scroll-progress: 0;
        }

        body {
            font-family: Inter, sans-serif;
            background-color: #F1FAEE; /* Lightest color from palette */
            margin: 0;
            overflow-x: hidden;
            transition: overflow 0.3s ease-in-out;
            scroll-behavior: smooth; /* Smooth scrolling for anchor links and programmatic scrolls */
        }
        
        /* A class to prevent scrolling when the product page is open */
        .no-scroll {
            overflow: hidden;
        }

        /* Custom styling for the header section */
        .header-container {
            width: 100%;
            height: 300px;
            display: flex;
            justify-content: center;
            align-items: center;
            text-align: center;
            position: relative;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
            /* Updated heading style with canopy effect using new blues */
            background:
                radial-gradient(circle at 50% 100%, transparent 25px, #F1FAEE 26px) 0 100% / 50px 50px repeat-x,
                linear-gradient(135deg, #457B9D, #1D3557) 0 0 / 100% 100% no-repeat; /* Primary blue to dark blue */
        }

        .header-text {
            font-family: Inter, sans-serif; /* Explicitly set font */
            font-size: 6rem; /* Toned down font size */
            font-weight: 900;
            color: #ffffff;
            z-index: 10;
            transform: scaleY(1.4);
            transform-origin: center;
        }
        
        /* Responsive design for smaller screens */
        @media (max-width: 768px) {
            .header-text {
                font-size: 3.5rem; /* Adjusted for mobile */
                transform: scaleY(1.4);
            }
        }
        
        /* Frosted glass card styling */
        .frosted-glass-card {
            background-color: rgba(255, 255, 255, 0.6);
            backdrop-filter: blur(8px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 1.5rem;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1), 0 10px 15px rgba(0, 0, 0, 0.05);
            transition: all 0.3s ease-in-out; /* Smoother hover animation for all properties */
        }
        .frosted-glass-card:hover {
            transform: translateY(-5px); /* Slightly more pronounced bounce */
            box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1), 0 25px 50px rgba(0, 0, 0, 0.05);
        }
        
        /* New styling for the gradient heading */
        .gradient-text {
            font-family: Inter, sans-serif; /* Explicitly set font */
            background-image: linear-gradient(to right, #457B9D, #A8DADC); /* Primary blue to light blue */
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            color: transparent;
            font-size: 2rem; /* Toned down font size */
            font-weight: 800; /* Extra bold */
            line-height: 1.2;
        }

        @media (min-width: 768px) {
            .gradient-text {
                font-size: 3rem; /* Toned down font size */
            }
        }

        /* Full-screen product view styles */
        .product-page-container {
            position: fixed;
            top: 0; 
            left: 0;
            width: 100%;
            height: 100vh; 
            background-color: #F1FAEE; /* Match body background */
            z-index: 40;
            visibility: hidden;
            opacity: 0;
            /* Set transform origin to top-left for correct zoom effect */
            transform-origin: 0 0;
            /* Initial state: scaled down and positioned over the product card */
            transform: translate(var(--initial-x, 0), var(--initial-y, 0)) scale(var(--initial-scale-x, 1), var(--initial-scale-y, 1));
            /* Smoother animation transition for both transform and opacity */
            transition: transform 0.5s cubic-bezier(0.68, -0.55, 0.27, 1.55), opacity 0.5s ease-in-out, border-radius 0.5s ease-in-out;
            
            /* Apply rounded corners for the initial and final state */
            border-radius: var(--initial-border-radius, 1.5rem);
            overflow-y: auto; /* Allow scrolling for product details */
        }

        .product-page-container.visible {
            visibility: visible;
            opacity: 1;
            /* Final state: full-screen with no scaling */
            transform: translate(0, 0) scale(1, 1);
            border-radius: 0;
        }
        
        .product-card {
            cursor: pointer;
            transition: opacity 0.5s ease-in-out, transform 0.3s ease-in-out, box-shadow 0.3s ease-in-out; /* Add smooth transitions */
        }

        /* Style for the back to products button */
        .back-button {
            padding: 1rem 1.5rem;
            background-color: #A8DADC; /* Light blue from palette */
            color: #1D3557; /* Dark blue text */
            border-radius: 9999px;
            font-weight: bold;
            display: flex;
            align-items: center;
            gap: 0.5rem;
            box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
            transition: background-color 0.2s, box-shadow 0.2s;
        }
        
        .back-button:hover {
            background-color: #8FD1D4; /* Slightly darker light blue */
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
        }
        
        /* Scroll to top button styling */
        #scroll-to-top-btn {
            position: fixed;
            bottom: 2rem;
            right: 2rem;
            width: 3.5rem;
            height: 3.5rem;
            background-color: transparent; /* Make background transparent by default */
            color: #1D3557; /* Dark blue text */
            border-radius: 9999px; /* Make it a circle */
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
            z-index: 50; /* Ensure it's above product page but below chatbot */
            opacity: 0;
            transform: translateY(200%);
            transition: all 0.3s cubic-bezier(0.4, 0.0, 0.2, 1); /* Smoother M3 animation */
            overflow: hidden;
        }
        
        #scroll-to-top-btn:hover {
            transform: translateY(0) scale(1.05);
            background-color: rgba(255, 255, 255, 0.8); /* Add a background on hover */
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
        }
        
        #scroll-to-top-btn.visible {
            opacity: 1;
            transform: translateY(0%);
        }
        
        /* Progress ring styling using conic-gradient for a smooth fill */
        #scroll-to-top-btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            border-radius: 50%;
            background: conic-gradient(
                #457B9D calc(var(--scroll-progress) * 1%), /* Primary blue for progress */
                #E5E7EB calc(var(--scroll-progress) * 1%)
            );
            transition: background 0.2s linear; /* Smoother progress animation */
            z-index: -1;
        }

        /* Sticky table header for comparison chart */
        .comparison-table th {
            font-family: Inter, sans-serif; /* Explicitly set font */
            position: sticky;
            top: 0; /* Sticks to the top of the scrolling container */
            background-color: #A8DADC; /* Light blue for sticky header */
            z-index: 10; /* Ensure it stays above scrolling content */
            box-shadow: 0 2px 4px rgba(0,0,0,0.05); /* Subtle shadow for depth */
            border-radius: 0.75rem 0.75rem 0 0; /* Rounded top corners */
        }
        
        /* Added for larger table cells */
        .comparison-table td, .comparison-table th {
            padding: 1rem 1.5rem; /* Larger padding for bigger cells */
            font-size: 1rem; /* Slightly larger font for readability */
        }
        @media (min-width: 768px) {
            .comparison-table td, .comparison-table th {
                padding: 1.25rem 2rem; /* Even larger padding on desktop */
                font-size: 1.125rem; /* Larger font on desktop */
            }
        }
        .comparison-table {
            border-collapse: separate; /* Needed for border-radius on cells */
            border-spacing: 0 0.5rem; /* Space between rows */
        }
        .comparison-table tbody tr {
            background-color: rgba(255, 255, 255, 0.8); /* Slightly more opaque for rows */
            border-radius: 0.75rem; /* Rounded rows */
            box-shadow: 0 1px 3px rgba(0,0,0,0.05); /* Subtle shadow for rows */
            transition: all 0.2s ease-in-out;
        }
        .comparison-table tbody tr:hover {
            background-color: rgba(255, 255, 255, 1);
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
            transform: translateY(-2px);
        }
        .comparison-table tbody tr td:first-child {
            border-top-left-radius: 0.75rem;
            border-bottom-left-radius: 0.75rem;
        }
        .comparison-table tbody tr td:last-child {
            border-top-right-radius: 0.75rem;
            border-bottom-right-radius: 0.75rem;
        }


        /* AI Chatbot styles - Material 3 Inspired */
        #ai-chatbot-container {
            position: fixed;
            bottom: 2rem; /* Aligned with scroll-to-top button */
            left: 2rem; /* Positioned on the left */
            z-index: 60; /* Higher z-index than scroll-to-top button */
            display: flex;
            flex-direction: column;
            align-items: flex-start;
            pointer-events: none;
        }

        #ai-chatbot-toggle-btn {
            width: 3.5rem;
            height: 3.5rem;
            background-color: #457B9D; /* Primary blue for the AI button */
            color: #ffffff;
            border-radius: 1rem; /* M3-style rounded corners */
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
            transition: all 0.3s cubic-bezier(0.4, 0.0, 0.2, 1); /* Smoother M3 animation */
            pointer-events: auto;
        }

        #ai-chatbot-toggle-btn:hover {
            transform: scale(1.05);
            background-color: #1D3557; /* Darker blue on hover */
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
        }

        #ai-chat-window {
            background-color: #F1FAEE; /* Match body background */
            border: 1px solid rgba(0, 0, 0, 0.05);
            border-radius: 1.75rem; /* Large, modern border radius */
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1); /* More diffused shadow */
            width: 320px; /* Slightly wider */
            height: 450px; /* Slightly taller */
            display: flex;
            flex-direction: column;
            margin-bottom: 1rem;
            overflow: hidden;
            transform: translateY(50px) scale(0.95);
            opacity: 0;
            visibility: hidden;
            transition: all 0.4s cubic-bezier(0.4, 0.0, 0.2, 1); /* Emphasized M3 easing */
            transform-origin: bottom left;
            pointer-events: none;
        }

        #ai-chat-window.open {
            transform: translateY(0) scale(1);
            opacity: 1;
            visibility: visible;
            pointer-events: auto;
        }

        #ai-chat-header {
            font-family: Inter, sans-serif; /* Explicitly set font */
            background-color: #1D3557; /* Dark blue for header */
            color: white;
            padding: 1rem 1.25rem;
            border-top-left-radius: 1.75rem; /* Match window border-radius */
            border-top-right-radius: 1.75rem; /* Match window border-radius */
            font-weight: 600; /* Semibold */
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        #ai-chat-messages {
            flex-grow: 1;
            padding: 1rem;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 0.75rem; /* Space between messages */
        }

        #ai-chat-input-container {
            padding: 0.75rem;
            border-top: 1px solid #e0e0e0;
            display: flex;
            gap: 0.5rem;
            background-color: #F1FAEE; /* Slightly different background for input area */
        }

        #ai-chat-input {
            flex-grow: 1;
            padding: 0.75rem 1rem;
            border: 1px solid #A8DADC; /* Light blue border */
            border-radius: 1.5rem; /* Fully rounded input */
            font-size: 0.9rem;
            font-family: Inter, sans-serif;
            background-color: white;
        }
        #ai-chat-input:focus {
            outline: none;
            border-color: #457B9D; /* Primary blue on focus */
            box-shadow: 0 0 0 2px rgba(69, 123, 157, 0.2); /* Blue shadow */
        }

        #ai-chat-send-btn {
            background-color: #457B9D; /* Primary blue for send button */
            color: white;
            padding: 0.75rem;
            border-radius: 9999px; /* Circular button */
            cursor: pointer;
            transition: background-color 0.2s;
            font-family: Inter, sans-serif;
            font-weight: bold;
            display: flex;
            align-items: center;
            justify-content: center;
            width: 44px;
            height: 44px;
        }

        #ai-chat-send-btn:hover {
            background-color: #1D3557; /* Darker blue on hover */
        }

        .ai-message {
            background-color: #A8DADC; /* Light blue for AI message */
            color: #1D3557; /* Dark blue text */
            padding: 0.75rem 1rem;
            border-radius: 1.25rem;
            border-bottom-left-radius: 0.25rem; /* M3-style message bubble */
            align-self: flex-start;
            max-width: 85%;
            font-family: Inter, sans-serif;
        }

        .user-message {
            background-color: #E0EBF0; /* Slightly darker than background for user message */
            color: #1D3557; /* Dark blue text */
            padding: 0.75rem 1rem;
            border-radius: 1.25rem;
            border-bottom-right-radius: 0.25rem; /* M3-style message bubble */
            align-self: flex-end;
            text-align: left;
            max-width: 85%;
            font-family: Inter, sans-serif;
        }
        .ai-message p, .user-message p {
            margin: 0;
            word-wrap: break-word;
        }

        /* NEW: Typing indicator animation */
        .typing-indicator {
            display: flex;
            align-items: center;
            gap: 0.25rem;
        }
        .typing-indicator span {
            height: 8px;
            width: 8px;
            background-color: #9ca3af;
            border-radius: 50%;
            animation: bounce 1.4s infinite ease-in-out both;
        }
        .typing-indicator span:nth-child(1) {
            animation-delay: -0.32s;
        }
        .typing-indicator span:nth-child(2) {
            animation-delay: -0.16s;
        }
        @keyframes bounce {
            0%, 80%, 100% {
                transform: scale(0);
            }
            40% {
                transform: scale(1.0);
            }
        }
        
        /* Fix for countdown timer shifting text */
        #countdown-timer {
            font-variant-numeric: tabular-nums;
        }
    </style>
</head>
<body>

    <!-- Sticky Announcement Banner -->
    <div id="announcement-banner" class="bg-[#E63946] text-white text-sm font-bold text-center py-2 px-4 sticky top-0 z-50 shadow-md">
        BACK TO SCHOOL SALE! ALL ITEMS 15% OFF! <span id="countdown-timer"></span>
    </div>

    <!-- Combined Header Section -->
    <div id="header" class="header-container canopy-header">
        <h1 class="header-text">GMarketplace</h1>
    </div>

    <!-- Product Grid Section - Now in its own frosted-glass-card section -->
    <div id="product-list-section" class="max-w-6xl mx-auto py-4 px-4 sm:px-6 lg:px-8"> <!-- Reduced vertical padding -->
        <div class="frosted-glass-card p-8 md:p-12 text-center">
            <h2 class="gradient-text mb-2">Today's Limited Deals</h2>
            <p class="text-center text-gray-500 text-sm mb-8">Updated regularly, check back later for more deals</p>
            <div id="product-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
                <!-- Product cards will be dynamically inserted here by JavaScript -->
            </div>
        </div>
    </div>

    <!-- Payment & Ordering Section (Now on the main page) -->
    <div id="payment-section" class="max-w-6xl mx-auto py-4 px-4 sm:px-6 lg:px-8"> <!-- Reduced vertical padding -->
        <div class="frosted-glass-card p-8 md:p-12 text-center">
            <h2 class="text-3xl font-bold text-gray-800 mb-4 font-inter">Payment & Ordering</h2>
            <p class="text-gray-600 leading-relaxed mb-6 font-inter">
                All purchases are currently processed through a secure checkout link. Click the button below to get started. 
                We will be adding more payment options soon!
            </p>
            <!-- Adjusted button sizes and colors -->
            <div class="flex flex-col sm:flex-row justify-center gap-4">
                <button class="bg-[#457B9D] text-white font-bold py-3 px-8 rounded-full hover:bg-[#1D3557] transition-colors duration-300 w-full sm:w-auto text-lg shadow-md font-inter">
                    Purchase via Google Forms
                </button>
                <button class="bg-[#A8DADC] text-[#1D3557] font-bold py-3 px-8 rounded-full hover:bg-[#8FD1D4] transition-colors duration-300 w-full sm:w-auto text-lg shadow-md font-inter">
                    DM us on Instagram to buy
                </button>
            </div>
        </div>
    </div>

    <!-- New Customer Service Section -->
    <div id="customer-service-section" class="max-w-6xl mx-auto py-4 px-4 sm:px-6 lg:px-8"> <!-- Reduced vertical padding -->
        <div class="frosted-glass-card p-8 md:p-12 text-center">
            <h2 class="text-3xl font-bold text-gray-800 mb-4 font-inter">Questions or Concerns?</h2>
            <p class="text-gray-600 leading-relaxed mb-6 font-inter">
                If you have any questions about our products, an existing order, or need general assistance, please don't hesitate to reach out. We are here to help!
            </p>
            <!-- Adjusted button sizes and colors -->
            <div class="flex flex-col sm:flex-row justify-center gap-4">
                <a href="mailto:your.email@example.com" class="bg-[#D62828] text-white font-bold py-3 px-8 rounded-full hover:bg-[#A32020] transition-colors duration-300 w-full sm:w-auto text-lg shadow-md font-inter">
                    Contact us via email
                </a>
                <button class="bg-[#A8DADC] text-[#1D3557] font-bold py-3 px-8 rounded-full hover:bg-[#8FD1D4] transition-colors duration-300 w-full sm:w-auto text-lg shadow-md font-inter">
                    Contact us via Instagram
                </button>
            </div>
        </div>
    </div>

    <!-- About Us Section -->
    <div id="about-us-section" class="max-w-6xl mx-auto py-4 px-4 sm:px-6 lg:px-8"> <!-- Reduced vertical padding -->
        <div class="frosted-glass-card p-8 md:p-12 text-center">
            <h2 class="text-3xl font-bold text-gray-800 mb-4 font-inter">About GMarketplace</h2>
            <p class="text-gray-600 leading-relaxed font-inter">
                Welcome to GMarketplace, your go-to destination for high-quality products at unbeatable prices. 
                We believe in offering a wide variety of items that are both durable and stylish. Our products are
                carefully selected to ensure they meet our strict quality standards. We are committed to
                providing you with an exceptional shopping experience and products you'll love.
            </p>
            <p class="text-gray-500 text-sm mt-4 font-inter">An OurTeam Brand</p>
            <p class="text-gray-500 text-sm font-inter">&copy; OurTeam Inc.</p>
        </div>
    </div>

    <!-- Full-screen product page container (initially hidden) -->
    <div id="product-page" class="product-page-container">
        <div class="container mx-auto p-8 lg:p-12 pt-[var(--banner-height)]"> <!-- Added padding-top to account for banner -->
            <!-- Back to products button -->
            <button id="close-btn" class="back-button mb-8">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18" />
                </svg>
                Back to Products
            </button>
            
            <div id="product-details-content">
                <!-- Product details will be dynamically inserted here by JavaScript -->
            </div>
        </div>
    </div>

    <!-- Scroll to top button with progress indicator -->
    <div id="scroll-to-top-btn">
        <span class="material-symbols-outlined text-3xl">
            arrow_upward
        </span>
    </div>

    <!-- AI Chatbot Container -->
    <div id="ai-chatbot-container">
        <div id="ai-chat-window">
            <div id="ai-chat-header">
                AI Customer Service
                <span id="ai-chat-close-btn" class="material-symbols-outlined cursor-pointer">
                    close
                </span>
            </div>
            <div id="ai-chat-messages">
                <div class="ai-message"><p>Hello! Welcome to GMarketplace! How can I assist you with our products today?</p></div>
            </div>
            <div id="ai-chat-input-container">
                <input type="text" id="ai-chat-input" placeholder="Ask a question...">
                <button id="ai-chat-send-btn">
                    <span class="material-symbols-outlined text-xl">
                        send
                    </span>
                </button>
            </div>
        </div>
        <button id="ai-chatbot-toggle-btn">
            <span class="material-symbols-outlined text-3xl">
                chat
            </span>
            <span class="sr-only">Open Chatbot</span>
        </button>
    </div>

    <script>
        // Use a JavaScript array to store product data. This makes it easy to add or edit products.
        const products = [
            {
                id: 'prod1',
                name: 'Replica AirPods Pro',
                image: 'https://placehold.co/600x400/2F3136/FFFFFF/png?text=AirPods+Pro',
                description: 'Experience premium audio with our high-quality replica AirPods Pro. Enjoy crystal-clear sound and comfortable fit.',
                price: 'NT$2,000',
                originalPrice: 'NT$2,350', // Original price for display
                stock: '1 Left!',
                isNew: true, // New flag for the new tag
                chartData: [
                    { feature: 'Active Noise Cancellation', real: 'check_circle', replica: 'cancel' },
                    { feature: 'Transparency Mode', real: 'check_circle', replica: 'cancel' },
                    { feature: 'Spatial Audio', real: 'check_circle', replica: 'cancel' },
                    { feature: 'Adaptive EQ', real: 'check_circle', replica: 'cancel' },
                    { feature: 'Customizable Fit (Silicone Tips)', real: 'check_circle', replica: 'check_circle' },
                    { feature: 'Battery Life (Earbuds)', real: 'check_circle', replica: 'cancel' },
                    { feature: 'Battery Life (Case)', real: 'check_circle', replica: 'cancel' },
                    { feature: 'Wireless Charging Case', real: 'check_circle', replica: 'check_circle' },
                    { feature: 'H1 Chip Features', real: 'check_circle', replica: 'cancel' },
                    { feature: 'Find My Support', real: 'check_circle', replica: 'cancel' },
                    { feature: 'Audio Sharing', real: 'check_circle', replica: 'cancel' },
                    { feature: 'Sweat and Water Resistant', real: 'check_circle', replica: 'cancel' },
                    { feature: 'Sound Quality', real: 'check_circle', replica: 'check_circle' },
                    { feature: 'Microphone Quality', real: 'check_circle', replica: 'cancel' },
                ]
            },
            {
                id: 'prod2',
                name: 'Replica AirPods 4',
                image: 'https://placehold.co/600x400/2F3136/FFFFFF/png?text=AirPods+4',
                description: 'Introducing our Replica AirPods 4, delivering clear audio and seamless connectivity for your everyday needs.',
                price: 'NT$2,000',
                originalPrice: 'NT$2,350', // Original price for display
                stock: '1 Left!', 
                isNew: true,
                chartData: [
                    { feature: 'Active Noise Cancellation', real: 'cancel', replica: 'cancel' },
                    { feature: 'Transparency Mode', real: 'cancel', replica: 'cancel' },
                    { feature: 'Spatial Audio', real: 'cancel', replica: 'cancel' },
                    { feature: 'Adaptive EQ', real: 'cancel', replica: 'cancel' },
                    { feature: 'Customizable Fit (Silicone Tips)', real: 'cancel', replica: 'check_circle' },
                    { feature: 'Battery Life (Earbuds)', real: 'check_circle', replica: 'check_circle' },
                    { feature: 'Battery Life (Case)', real: 'check_circle', replica: 'check_circle' },
                    { feature: 'Wireless Charging Case', real: 'check_circle', replica: 'check_circle' },
                    { feature: 'H1 Chip Features', real: 'check_circle', replica: 'cancel' },
                    { feature: 'Find My Support', real: 'check_circle', replica: 'cancel' },
                    { feature: 'Audio Sharing', real: 'check_circle', replica: 'cancel' },
                    { feature: 'Sweat and Water Resistant', real: 'check_circle', replica: 'cancel' },
                    { feature: 'Sound Quality', real: 'check_circle', replica: 'check_circle' },
                    { feature: 'Microphone Quality', real: 'check_circle', replica: 'check_circle' },
                ]
            },
            {
                id: 'prod3',
                name: 'REAL Lenovo X13 Yoga Thinkpad Stylus',
                image: 'https://placehold.co/600x400/2F3136/FFFFFF/png?text=Lenovo+Stylus',
                description: 'Experience precision with the REAL Lenovo X13 Yoga Thinkpad Stylus. Perfect for drawing, note-taking, and navigating your device.',
                price: 'NT$700',
                originalPrice: 'NT$824', // Calculated 15% off: 700 / (1 - 0.15) = 823.53
                stock: '1 Left!',
                isNew: true,
                chartData: null // No comparison chart for real product
            },
            {
                id: 'prod4',
                name: 'International Candy Box',
                image: 'https://placehold.co/600x400/2F3136/FFFFFF/png?text=Candy+Box',
                description: 'A delightful assortment of international candies, from Japanese Senbei to Hi-Chew and other international treats.',
                price: 'NT$150',
                originalPrice: 'NT$177', // Calculated 15% off: 150 / (1 - 0.15) = 176.47
                stock: '1 Left!',
                isNew: true,
                chartData: null // No comparison chart for this product
            }
        ];

        // Global variable to store the original card's position for the animation
        let lastClickedCard = null;
        let isAnimating = false; // Flag to prevent multiple animations

        // Get DOM elements
        const body = document.body;
        const productGrid = document.getElementById('product-grid');
        const productPage = document.getElementById('product-page');
        const closeBtn = document.getElementById('close-btn');
        const productDetailsContent = document.getElementById('product-details-content');
        const announcementBanner = document.getElementById('announcement-banner');
        const scrollToTopBtn = document.getElementById('scroll-to-top-btn');
        
        // AI Chatbot elements
        const aiChatbotContainer = document.getElementById('ai-chatbot-container');
        const aiChatbotToggleBtn = document.getElementById('ai-chatbot-toggle-btn');
        const aiChatWindow = document.getElementById('ai-chat-window');
        const aiChatCloseBtn = document.getElementById('ai-chat-close-btn');
        const aiChatMessages = document.getElementById('ai-chat-messages');
        const aiChatInput = document.getElementById('ai-chat-input');
        const aiChatSendBtn = document.getElementById('ai-chat-send-btn');


        // Get the computed height of the announcement banner
        const bannerHeight = announcementBanner.offsetHeight;
        document.documentElement.style.setProperty('--banner-height', `${bannerHeight}px`);

        // Function to create a product card element
        const createProductCard = (product) => {
            const card = document.createElement('div');
            card.className = 'frosted-glass-card overflow-hidden relative product-card flex flex-col'; // Added flex and flex-col
            card.setAttribute('data-id', product.id);
            
            const newTag = `<div class="absolute top-3 right-3 bg-[#A8DADC] text-[#1D3557] text-xs font-bold px-3 py-1 rounded-full font-inter">✨ New</div>`;
            
            card.innerHTML = `
                <div class="relative">
                    <div class="absolute top-3 left-3 flex flex-col items-start gap-1 z-10">
                        <div class="bg-[#E63946] text-white text-xs font-bold px-3 py-1 rounded-full font-inter">1 Left!</div>
                        <div class="bg-green-400 text-green-900 text-xs font-bold px-3 py-1 rounded-full font-inter">15% Off!</div>
                    </div>
                    ${newTag}
                    <img src="${product.image}" onerror="this.onerror=null;this.src='https://placehold.co/600x400';" alt="${product.name}" class="w-full h-48 object-cover">
                </div>
                <div class="p-6 flex flex-col flex-grow"> <!-- Added flex-grow to this div -->
                    <div class="flex-grow">
                        <h2 class="text-xl font-bold text-gray-900 font-inter">${product.name}</h2>
                        <p class="mt-2 text-gray-600 line-clamp-2 font-inter">${product.description}</p>
                    </div>
                    <div class="flex items-center justify-center mt-4">
                        <button class="bg-[#457B9D] text-white font-bold py-3 px-8 rounded-full hover:bg-[#1D3557] transition-colors duration-300 w-full text-lg shadow-md font-inter">
                            View Product
                        </button>
                    </div>
                </div>
            `;
            return card;
        };

        // Function to render the product grid
        const renderProductGrid = () => {
            productGrid.innerHTML = '';
            products.forEach(product => {
                const card = createProductCard(product);
                productGrid.appendChild(card);
            });
            attachEventListeners();
        };

        // Function to generate the comparison chart HTML
        const generateComparisonChartHtml = (chartData) => {
            if (!chartData || chartData.length === 0) return '';
            let chartHtml = `
                <div class="frosted-glass-card p-6 md:p-8 mt-12 text-center">
                    <h3 class="text-2xl font-bold text-gray-800 mb-6 font-inter">Feature Comparison: Real vs. Our Replicas</h3>
                    <div class="overflow-x-auto">
                        <table class="min-w-full w-full bg-white/70 rounded-lg shadow-md comparison-table">
                            <thead>
                                <tr class="bg-[#A8DADC] text-[#1D3557] uppercase text-base leading-normal">
                                    <th class="py-4 px-8 text-left rounded-tl-lg font-inter">Feature</th>
                                    <th class="py-4 px-8 text-center font-inter">Real</th>
                                    <th class="py-4 px-8 text-center rounded-tr-lg font-inter">Our Replicas</th>
                                </tr>
                            </thead>
                            <tbody class="text-gray-600 text-base font-light">
            `;
            chartData.forEach(row => {
                chartHtml += `
                                <tr class="border-b border-gray-200 hover:bg-gray-100">
                                    <td class="py-3 px-6 text-left whitespace-nowrap font-inter">${row.feature}</td>
                                    <td class="py-3 px-6 text-center">
                                        <span class="material-symbols-outlined ${row.real === 'check_circle' ? 'text-green-600' : 'text-red-600'}">${row.real}</span>
                                    </td>
                                    <td class="py-3 px-6 text-center">
                                        <span class="material-symbols-outlined ${row.replica === 'check_circle' ? 'text-green-600' : 'text-red-600'}">${row.replica}</span>
                                    </td>
                                </tr>
                `;
            });
            chartHtml += `
                            </tbody>
                        </table>
                    </div>
                </div>
            `;
            return chartHtml;
        };

        // Function to get the HTML for the Payment & Ordering section
        const getPaymentSectionHtml = () => `
            <div class="frosted-glass-card p-8 md:p-12 text-center">
                <h2 class="text-3xl font-bold text-gray-800 mb-4 font-inter">Payment & Ordering</h2>
                <p class="text-gray-600 leading-relaxed mb-6 font-inter">All purchases are currently processed through a secure checkout link. Click the button below to get started. We will be adding more payment options soon!</p>
                <div class="flex flex-col sm:flex-row justify-center gap-4">
                    <button class="bg-[#457B9D] text-white font-bold py-3 px-8 rounded-full hover:bg-[#1D3557] transition-colors duration-300 w-full sm:w-auto text-lg shadow-md font-inter">Purchase via Google Forms</button>
                    <button class="bg-[#A8DADC] text-[#1D3557] font-bold py-3 px-8 rounded-full hover:bg-[#8FD1D4] transition-colors duration-300 w-full sm:w-auto text-lg shadow-md font-inter">DM us on Instagram to buy</button>
                </div>
            </div>
        `;

        // Function to get the HTML for the Customer Service section
        const getCustomerServiceSectionHtml = () => `
            <div class="frosted-glass-card p-8 md:p-12 text-center">
                <h2 class="text-3xl font-bold text-gray-800 mb-4 font-inter">Questions or Concerns?</h2>
                <p class="text-gray-600 leading-relaxed mb-6 font-inter">If you have any questions about our products, an existing order, or need general assistance, please don't hesitate to reach out. We are here to help!</p>
                <div class="flex flex-col sm:flex-row justify-center gap-4">
                    <a href="mailto:your.email@example.com" class="bg-[#D62828] text-white font-bold py-3 px-8 rounded-full hover:bg-[#A32020] transition-colors duration-300 w-full sm:w-auto text-lg shadow-md font-inter">Contact us via email</a>
                    <button class="bg-[#A8DADC] text-[#1D3557] font-bold py-3 px-8 rounded-full hover:bg-[#8FD1D4] transition-colors duration-300 w-full sm:w-auto text-lg shadow-md font-inter">Contact us via Instagram</button>
                </div>
            </div>
        `;

        // Function to display product details in the full-screen page
        const showProductDetails = (productId, clickedCard) => {
            const product = products.find(p => p.id === productId);
            if (!product || isAnimating) return;

            isAnimating = true;
            lastClickedCard = clickedCard;
            const cardRect = clickedCard.getBoundingClientRect();
            const viewportWidth = window.innerWidth;
            const viewportHeight = window.innerHeight;

            const scaleX = cardRect.width / viewportWidth;
            const scaleY = cardRect.height / viewportHeight;
            const translateX = cardRect.left;
            const translateY = cardRect.top;

            productPage.style.setProperty('--initial-x', `${translateX}px`);
            productPage.style.setProperty('--initial-y', `${translateY}px`);
            productPage.style.setProperty('--initial-scale-x', `${scaleX}`);
            productPage.style.setProperty('--initial-scale-y', `${scaleY}`);
            productPage.style.setProperty('--initial-border-radius', '1.5rem');
            
            productPage.style.visibility = 'visible';
            clickedCard.style.opacity = '0';

            productDetailsContent.innerHTML = `
                <div class="frosted-glass-card p-6 md:p-8 flex flex-col lg:flex-row gap-8 items-center">
                    <img src="${product.image}" onerror="this.onerror=null;this.src='https://placehold.co/800x600';" alt="${product.name}" class="w-full lg:w-1/2 h-64 lg:h-auto object-cover rounded-lg shadow-md">
                    <div class="flex-1 text-center lg:text-left">
                        <h2 class="text-3xl lg:text-4xl font-extrabold text-[#1D3557] mb-4 font-inter">${product.name}</h2>
                        <div class="flex flex-wrap gap-2 justify-center lg:justify-start mb-4">
                            <div class="bg-[#E63946] text-white text-xs font-bold px-3 py-1 rounded-full font-inter">1 Left!</div>
                            <div class="bg-green-400 text-green-900 text-xs font-bold px-3 py-1 rounded-full font-inter">15% Off!</div>
                            <div class="bg-[#A8DADC] text-[#1D3557] text-xs font-bold px-3 py-1 rounded-full font-inter">✨ New</div>
                        </div>
                        <p class="text-2xl font-bold text-[#457B9D] mb-2 font-inter">${product.price} <span class="text-gray-400 line-through text-lg ml-2 font-inter">${product.originalPrice}</span></p>
                        <p class="text-gray-700 text-lg leading-relaxed mb-8 font-inter">${product.description}</p>
                    </div>
                </div>
                ${product.chartData ? generateComparisonChartHtml(product.chartData) : ''}
                <div class="max-w-6xl mx-auto py-8 px-4 sm:px-6 lg:px-8">${getPaymentSectionHtml()}</div>
                <div class="max-w-6xl mx-auto py-8 px-4 sm:px-6 lg:px-8">${getCustomerServiceSectionHtml()}</div>
            `;

            requestAnimationFrame(() => {
                productPage.classList.add('visible');
                body.classList.add('no-scroll');
            });

            productPage.scrollTop = 0;
            productPage.addEventListener('scroll', updateProductPageScrollProgress);
            
            setTimeout(() => {
                isAnimating = false;
            }, 500); // Animation duration
        };

        // Function to hide the product details page
        const hideProductDetails = () => {
            if (!lastClickedCard || isAnimating) return;

            isAnimating = true;
            const cardRect = lastClickedCard.getBoundingClientRect();
            const viewportWidth = window.innerWidth;
            const viewportHeight = window.innerHeight;

            const scaleX = cardRect.width / viewportWidth;
            const scaleY = cardRect.height / viewportHeight;
            const translateX = cardRect.left;
            const translateY = cardRect.top;

            // Explicitly set the transform to the card's position before removing the 'visible' class
            productPage.style.transform = `translate(${translateX}px, ${translateY}px) scale(${scaleX}, ${scaleY})`;
            productPage.style.borderRadius = '1.5rem';
            productPage.classList.remove('visible');

            // Use a timeout for cleanup to avoid transitionend issues and state conflicts
            setTimeout(() => {
                productPage.style.visibility = 'hidden';
                // CRITICAL FIX: Reset transform and border-radius to prevent issues on next open
                productPage.style.transform = '';
                productPage.style.borderRadius = '';

                if (lastClickedCard) {
                    lastClickedCard.style.opacity = '1';
                }
                body.classList.remove('no-scroll');
                productPage.removeEventListener('scroll', updateProductPageScrollProgress);
                lastClickedCard = null;
                isAnimating = false;
            }, 500); // Match CSS transition duration
        };

        // Event Listeners
        const attachEventListeners = () => {
            document.querySelectorAll('.product-card').forEach(card => {
                card.addEventListener('click', () => {
                    const productId = card.getAttribute('data-id');
                    showProductDetails(productId, card);
                });
            });
        };

        closeBtn.addEventListener('click', hideProductDetails);

        // Countdown Timer for Announcement Banner
        const countdownTimer = document.getElementById('countdown-timer');
        let countdownInterval;
        const setCountdown = () => {
            const now = new Date();
            const saleEndDate = new Date(now.getFullYear(), 8, 1); // September is month 8
            const timeLeft = saleEndDate.getTime() - now.getTime();

            if (timeLeft <= 0) {
                countdownTimer.textContent = " Sale Ended!";
                if(countdownInterval) clearInterval(countdownInterval);
                return;
            }

            const days = Math.floor(timeLeft / (1000 * 60 * 60 * 24));
            const hours = Math.floor((timeLeft % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
            const minutes = Math.floor((timeLeft % (1000 * 60 * 60)) / (1000 * 60));
            const seconds = Math.floor((timeLeft % (1000 * 60)) / 1000);
            const milliseconds = Math.floor((timeLeft % 1000) / 10);

            const formattedDays = String(days).padStart(2, '0');
            const formattedHours = String(hours).padStart(2, '0');
            const formattedMinutes = String(minutes).padStart(2, '0');
            const formattedSeconds = String(seconds).padStart(2, '0');
            const formattedMilliseconds = String(milliseconds).padStart(2, '0');

            countdownTimer.textContent = ` Ends in ${formattedDays}d ${formattedHours}h ${formattedMinutes}m ${formattedSeconds}s ${formattedMilliseconds}ms`;
        };

        setCountdown();
        countdownInterval = setInterval(setCountdown, 10);

        // Scroll-to-top button logic
        window.addEventListener('scroll', () => {
            if (window.scrollY > 200) {
                scrollToTopBtn.classList.add('visible');
            } else {
                scrollToTopBtn.classList.remove('visible');
            }
            const scrollHeight = document.documentElement.scrollHeight - window.innerHeight;
            const scrolled = window.scrollY;
            const progress = scrollHeight > 0 ? (scrolled / scrollHeight) * 100 : 0;
            document.documentElement.style.setProperty('--scroll-progress', progress);
        });

        const updateProductPageScrollProgress = () => {
            const scrollHeight = productPage.scrollHeight - productPage.clientHeight;
            const scrolled = productPage.scrollTop;
            const progress = scrollHeight > 0 ? (scrolled / scrollHeight) * 100 : 0;
            document.documentElement.style.setProperty('--scroll-progress', progress);
        };

        scrollToTopBtn.addEventListener('click', () => {
            const target = productPage.classList.contains('visible') ? productPage : window;
            target.scrollTo({ top: 0, behavior: 'smooth' });
        });

        // Simple Markdown to HTML converter for AI messages
        const markdownToHtml = (text) => text.replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>');

        // AI Chatbot Logic
        let chatHistory = [{ role: "model", parts: [{ text: "Hello! Welcome to GMarketplace! How can I assist you with our products today?" }] }];
        let isTyping = false;

        const renderMessage = (message, sender) => {
            const messageDiv = document.createElement('div');
            messageDiv.className = sender === 'user' ? 'user-message' : 'ai-message';
            messageDiv.innerHTML = `<p>${markdownToHtml(message)}</p>`; 
            aiChatMessages.appendChild(messageDiv);
            aiChatMessages.scrollTop = aiChatMessages.scrollHeight;
        };

        const showTypingIndicator = () => {
            if (isTyping) return;
            isTyping = true;
            const typingDiv = document.createElement('div');
            typingDiv.className = 'ai-message typing-indicator';
            typingDiv.innerHTML = `<span></span><span></span><span></span>`;
            aiChatMessages.appendChild(typingDiv);
            aiChatMessages.scrollTop = aiChatMessages.scrollHeight;
        };

        const hideTypingIndicator = () => {
            const typingDiv = aiChatMessages.querySelector('.typing-indicator');
            if (typingDiv) typingDiv.remove();
            isTyping = false;
        };

        const sendMessage = async () => {
            const userMessageText = aiChatInput.value.trim();
            if (userMessageText === '') return;

            renderMessage(userMessageText, 'user');
            chatHistory.push({ role: "user", parts: [{ text: userMessageText }] });
            aiChatInput.value = '';
            showTypingIndicator();

            try {
                const productDataForAI = products.map(p => ({
                    name: p.name, description: p.description, price: p.price, originalPrice: p.originalPrice, stock: p.stock
                }));

                const aiPrompt = `
                    You are a helpful and concise AI assistant for GMarketplace.
                    Your goal is to answer questions about products and website functionality.
                    **Crucially, only use information available on this website.** Do NOT invent information or external links.
                    Here are the products available on the website: ${JSON.stringify(productDataForAI, null, 2)}
                    If asked about purchasing, direct the user to the "Payment & Ordering" section on the main page.
                    If asked about support, direct the user to the "Questions or Concerns?" section on the main page.
                    Keep your responses short, to the point, and encourage follow-up questions.
                    Use Markdown for formatting, such as **bolding**.
                    Here is the current chat history:
                    ${chatHistory.map(msg => `${msg.role}: ${msg.parts[0].text}`).join('\n')}
                    User: ${userMessageText}
                    AI:
                `;

                const payload = { 
                    contents: [{ role: "user", parts: [{ text: aiPrompt }] }],
                    generationConfig: { temperature: 0.7 }
                };
                const apiKey = ""; // If you want to use models other than gemini-2.5-flash-preview-05-20 or imagen-3.0-generate-002, provide an API key here. Otherwise, leave this as-is.
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

                let response;
                let attempts = 0;
                const maxAttempts = 5;
                const baseDelay = 1000;

                while (attempts < maxAttempts) {
                    try {
                        response = await fetch(apiUrl, {
                            method: 'POST',
                            headers: { 'Content-Type': 'application/json' },
                            body: JSON.stringify(payload)
                        });
                        if (response.status === 429) {
                            const delay = baseDelay * Math.pow(2, attempts);
                            await new Promise(resolve => setTimeout(resolve, delay));
                            attempts++;
                            continue;
                        }
                        if (!response.ok) throw new Error(`HTTP error! status: ${response.status}`);
                        break;
                    } catch (error) {
                        if (attempts === maxAttempts - 1) throw error;
                        const delay = baseDelay * Math.pow(2, attempts);
                        await new Promise(resolve => setTimeout(resolve, delay));
                        attempts++;
                    }
                }

                const result = await response.json();
                hideTypingIndicator();

                if (result.candidates?.[0]?.content?.parts?.[0]) {
                    const aiResponseText = result.candidates[0].content.parts[0].text;
                    renderMessage(aiResponseText, 'ai');
                    chatHistory.push({ role: "model", parts: [{ text: aiResponseText }] });
                } else {
                    renderMessage("Sorry, our AI agent cannot help you at the moment. Please try again later. For urgent inquiries, please contact a human representitive using the links found on the main page.", 'ai');
                }
            } catch (error) {
                hideTypingIndicator();
                renderMessage("Sorry, our AI agent cannot help you at the moment. Please try again later. For urgent inquiries, please contact a human representitive using the links found on the main page.", 'ai');
                console.error("Error sending message to AI:", error);
            }
        };

        aiChatSendBtn.addEventListener('click', sendMessage);
        aiChatInput.addEventListener('keypress', (e) => { if (e.key === 'Enter') sendMessage(); });
        aiChatbotToggleBtn.addEventListener('click', () => { aiChatWindow.classList.toggle('open'); });
        aiChatCloseBtn.addEventListener('click', () => { aiChatWindow.classList.remove('open'); });

        // Initial render of products
        renderProductGrid();
    </script>
</body>
</html>
