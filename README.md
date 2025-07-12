<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Smart GST Calculator</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        :root {
            --primary: #4361ee;
            --secondary: #3a0ca3;
            --accent: #4895ef;
            --light: #f8f9fa;
            --dark: #212529;
            --success: #4cc9f0;
            --warning: #f72585;
            --card-bg: #ffffff;
            --shadow: rgba(0, 0, 0, 0.1);
            --gradient-start: #4361ee;
            --gradient-end: #3a0ca3;
            --gst-color: #4caf50;
        }

        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #e4edf5 100%);
            color: var(--dark);
            line-height: 1.6;
            min-height: 100vh;
            padding: 20px;
            position: relative;
        }

        .container {
            max-width: 1000px;
            margin: 0 auto;
        }

        header {
            text-align: center;
            padding: 20px 0;
            margin-bottom: 20px;
            position: relative;
        }

        .logo {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
            margin-bottom: 15px;
        }

        .logo i {
            background: var(--gst-color);
            color: white;
            width: 70px;
            height: 70px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
            font-size: 32px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
        }

        .logo h1 {
            font-size: 2.4rem;
            color: var(--gst-color);
            font-weight: 700;
            letter-spacing: -0.5px;
        }

        .subtitle {
            color: #6c757d;
            font-size: 1.2rem;
            max-width: 700px;
            margin: 10px auto 0;
            font-weight: 300;
        }

        .card {
            background: var(--card-bg);
            border-radius: 20px;
            box-shadow: 0 8px 30px rgba(0, 0, 0, 0.12);
            padding: 30px;
            margin-bottom: 30px;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }

        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 12px 40px rgba(0, 0, 0, 0.15);
        }

        .card-title {
            font-size: 1.6rem;
            color: var(--gst-color);
            margin-bottom: 25px;
            display: flex;
            align-items: center;
            gap: 12px;
            padding-bottom: 15px;
            border-bottom: 2px solid #f0f4f8;
        }

        .input-group {
            display: flex;
            flex-direction: column;
            gap: 20px;
            margin-bottom: 25px;
        }

        .input-row {
            display: flex;
            gap: 20px;
        }

        .input-column {
            flex: 1;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        label {
            font-weight: 600;
            color: #555;
            font-size: 1.05rem;
        }

        input, select {
            padding: 14px 18px;
            border: 2px solid #e2e8f0;
            border-radius: 12px;
            font-size: 1.05rem;
            background: #f8fafc;
            transition: all 0.3s;
        }

        input:focus, select:focus {
            outline: none;
            border-color: var(--accent);
            box-shadow: 0 0 0 4px rgba(72, 149, 239, 0.2);
        }

        .tabs {
            display: flex;
            gap: 12px;
            margin-bottom: 20px;
        }

        .tab {
            flex: 1;
            text-align: center;
            padding: 14px;
            background: #edf2f7;
            border-radius: 12px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s;
            font-size: 1.05rem;
        }

        .tab.active {
            background: var(--gst-color);
            color: white;
            box-shadow: 0 4px 10px rgba(76, 175, 80, 0.3);
        }

        .image-upload-container {
            border: 3px dashed #cbd5e0;
            border-radius: 15px;
            padding: 30px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
            position: relative;
            overflow: hidden;
            min-height: 200px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background: #f8fafc;
            margin-bottom: 20px;
        }

        .image-upload-container:hover {
            border-color: var(--gst-color);
            background: rgba(76, 175, 80, 0.05);
        }

        .image-upload-container i {
            font-size: 3.5rem;
            color: var(--gst-color);
            margin-bottom: 20px;
            transition: all 0.3s;
        }

        .image-upload-container:hover i {
            transform: translateY(-5px);
        }

        .image-upload-container p {
            margin-bottom: 12px;
            color: #4a5568;
            font-size: 1.1rem;
        }

        .image-upload-container .btn {
            background: var(--gst-color);
            color: white;
            padding: 10px 25px;
            border-radius: 10px;
            font-weight: 600;
            display: inline-block;
            transition: all 0.3s;
            font-size: 1.05rem;
        }

        .image-upload-container .btn:hover {
            background: #3d8b40;
            transform: translateY(-2px);
        }

        #imagePreview {
            max-width: 100%;
            max-height: 220px;
            margin-top: 20px;
            border-radius: 10px;
            display: none;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }

        .btn-container {
            display: flex;
            gap: 18px;
            margin-top: 15px;
        }

        .btn {
            flex: 1;
            padding: 16px;
            border: none;
            border-radius: 15px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
        }

        .btn-primary {
            background: var(--gst-color);
            color: white;
        }

        .btn-primary:hover {
            background: #3d8b40;
            transform: translateY(-3px);
            box-shadow: 0 6px 15px rgba(61, 139, 64, 0.3);
        }

        .btn-secondary {
            background: #e2e8f0;
            color: #4a5568;
        }

        .btn-secondary:hover {
            background: #cbd5e0;
            transform: translateY(-3px);
        }

        #response {
            min-height: 250px;
            padding: 25px;
            background: #f8fafc;
            border-radius: 15px;
            border: 2px solid #e2e8f0;
        }

        .response-content {
            line-height: 1.8;
            font-size: 1.1rem;
        }

        .response-content img {
            max-width: 100%;
            border-radius: 10px;
            margin: 15px 0;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }

        .loading {
            display: none;
            text-align: center;
            padding: 40px;
        }

        .loading i {
            font-size: 3rem;
            color: var(--gst-color);
            margin-bottom: 20px;
            animation: spin 1.5s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .ad-container {
            text-align: center;
            margin: 30px 0;
            padding: 20px;
            background: white;
            border-radius: 15px;
            box-shadow: 0 6px 15px rgba(0, 0, 0, 0.08);
        }

        .ad-label {
            font-size: 0.85rem;
            color: #718096;
            margin-bottom: 10px;
            text-transform: uppercase;
            letter-spacing: 1.5px;
            font-weight: 600;
        }

        footer {
            text-align: center;
            padding: 25px;
            color: #718096;
            font-size: 0.95rem;
            margin-top: 30px;
        }

        .error {
            background: #ffebee;
            color: #c62828;
            padding: 18px;
            border-radius: 12px;
            margin: 15px 0;
            border-left: 4px solid #c62828;
        }

        .result-container {
            background: #e8f5e9;
            border-radius: 12px;
            padding: 20px;
            margin: 20px 0;
            border-left: 4px solid var(--gst-color);
        }

        .result-title {
            font-weight: 700;
            color: var(--gst-color);
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .result-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
            margin-top: 20px;
        }

        .result-item {
            background: white;
            border-radius: 10px;
            padding: 15px;
            text-align: center;
            box-shadow: 0 4px 8px rgba(0,0,0,0.05);
        }

        .result-label {
            font-size: 0.9rem;
            color: #718096;
            margin-bottom: 5px;
        }

        .result-value {
            font-size: 1.4rem;
            font-weight: 700;
            color: var(--gst-color);
        }

        .gst-rate-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            margin-top: 10px;
        }

        .gst-rate {
            background: #e8f5e9;
            border: 2px solid #c8e6c9;
            border-radius: 10px;
            padding: 10px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: 600;
            color: #2e7d32;
        }

        .gst-rate.selected {
            background: var(--gst-color);
            color: white;
            border-color: var(--gst-color);
        }

        .gst-rate:hover {
            background: #c8e6c9;
        }

        @media (max-width: 768px) {
            .logo h1 {
                font-size: 1.9rem;
            }
            
            .card {
                padding: 25px;
            }
            
            .btn-container {
                flex-direction: column;
            }
            
            .tabs {
                flex-direction: column;
            }
            
            .image-upload-container {
                padding: 20px;
            }
            
            .input-row {
                flex-direction: column;
                gap: 15px;
            }
            
            .result-grid {
                grid-template-columns: 1fr;
            }
            
            .gst-rate-grid {
                grid-template-columns: repeat(2, 1fr);
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <div class="logo">
                <i class="fas fa-calculator"></i>
                <h1>Smart GST Calculator</h1>
            </div>
            <p class="subtitle">Calculate GST instantly or extract from receipts with AI</p>
            
            <div class="ad-container">
                <div class="ad-label">Advertisement</div>
                <div id="ad-banner">
                    <script type="text/javascript">
                        atOptions = { 
                            'key' : '286ac1ac73458ecfba3c2f9f53473f3e',
                            'format' : 'iframe',
                            'height' : 50,
                            'width' : 320,
                            'params' : {}
                        };
                    </script>
                    <script type="text/javascript" src="//www.highperformanceformat.com/286ac1ac73458ecfba3c2f9f53473f3e/invoke.js"></script>
                </div>
            </div>
        </header>
        
        <main>
            <div class="card">
                <h2 class="card-title"><i class="fas fa-calculator"></i> GST Calculator</h2>
                
                <div class="tabs">
                    <div class="tab active" data-tab="manual">Manual Calculation</div>
                    <div class="tab" data-tab="image">Scan Receipt</div>
                </div>
                
                <div class="input-group" id="manualTab">
                    <div class="input-row">
                        <div class="input-column">
                            <label for="amount">Amount (₹)</label>
                            <input type="number" id="amount" placeholder="Enter amount" value="1000">
                        </div>
                        <div class="input-column">
                            <label for="gstType">GST Type</label>
                            <select id="gstType">
                                <option value="inclusive">Inclusive of GST</option>
                                <option value="exclusive">Exclusive of GST</option>
                            </select>
                        </div>
                    </div>
                    
                    <div class="input-column">
                        <label>Select GST Rate</label>
                        <div class="gst-rate-grid">
                            <div class="gst-rate" data-rate="5">5%</div>
                            <div class="gst-rate" data-rate="12">12%</div>
                            <div class="gst-rate selected" data-rate="18">18%</div>
                            <div class="gst-rate" data-rate="28">28%</div>
                        </div>
                    </div>
                </div>
                
                <div class="input-group" id="imageTab" style="display: none;">
                    <div class="image-upload-container" id="dropArea">
                        <i class="fas fa-receipt"></i>
                        <p>Drag & drop your receipt here</p>
                        <p>OR</p>
                        <div class="btn">Select Receipt</div>
                        <input type="file" id="imageUpload" accept="image/*" style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; opacity: 0; cursor: pointer;">
                        <img id="imagePreview" alt="Preview">
                    </div>
                    
                    <div class="api-info">
                        <i class="fas fa-info-circle"></i> Using AI to extract GST details from your receipt
                    </div>
                </div>
                
                <div class="btn-container">
                    <button class="btn btn-primary" id="calculateBtn">
                        <i class="fas fa-calculator"></i> Calculate GST
                    </button>
                    <button class="btn btn-secondary" id="clearBtn">
                        <i class="fas fa-eraser"></i> Clear
                    </button>
                </div>
            </div>
            
            <div class="card">
                <h2 class="card-title"><i class="fas fa-file-invoice-dollar"></i> Calculation Results</h2>
                
                <div class="loading" id="loading">
                    <i class="fas fa-spinner"></i>
                    <p>Analyzing receipt. Please wait...</p>
                </div>
                
                <div id="response">
                    <div class="response-content" id="responseContent">
                        <div class="result-container">
                            <div class="result-title">
                                <i class="fas fa-calculator"></i> GST Calculation
                            </div>
                            <p>Enter amount and GST rate, then click "Calculate GST"</p>
                            <div class="result-grid">
                                <div class="result-item">
                                    <div class="result-label">Original Amount</div>
                                    <div class="result-value">₹0.00</div>
                                </div>
                                <div class="result-item">
                                    <div class="result-label">GST Amount</div>
                                    <div class="result-value">₹0.00</div>
                                </div>
                                <div class="result-item">
                                    <div class="result-label">Total Amount</div>
                                    <div class="result-value">₹0.00</div>
                                </div>
                                <div class="result-item">
                                    <div class="result-label">GST Rate</div>
                                    <div class="result-value">0%</div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            
            <div class="ad-container">
                <div class="ad-label">Advertisement</div>
                <div id="ad-banner-bottom">
                    <!-- Ad will be inserted by script -->
                </div>
            </div>
        </main>
        
        <footer>
            <p>© 2023 Smart GST Calculator | Powered by Google Gemini AI</p>
            <p>Disclaimer: This tool provides estimates only. Verify with a tax professional.</p>
        </footer>
    </div>
    
    <script>
        // Initialize the app when the page loads
        document.addEventListener('DOMContentLoaded', function() {
            // API Configuration - Using the latest Gemini model
            const API_KEY = "AIzaSyBTw0VtRGbNsvjRfTO1yuxV8VthX9qJ7Wg";
            const MODEL_NAME = "gemini-1.5-flash";
            
            // DOM Elements
            const manualTab = document.querySelector('[data-tab="manual"]');
            const imageTab = document.querySelector('[data-tab="image"]');
            const manualInputTab = document.getElementById('manualTab');
            const imageInputTab = document.getElementById('imageTab');
            const amountInput = document.getElementById('amount');
            const gstTypeSelect = document.getElementById('gstType');
            const gstRates = document.querySelectorAll('.gst-rate');
            const imageUpload = document.getElementById('imageUpload');
            const imagePreview = document.getElementById('imagePreview');
            const calculateBtn = document.getElementById('calculateBtn');
            const clearBtn = document.getElementById('clearBtn');
            const responseContent = document.getElementById('responseContent');
            const loading = document.getElementById('loading');
            const dropArea = document.getElementById('dropArea');
            
            // Current GST rate (default 18%)
            let currentGstRate = 18;
            
            // Set up tab switching
            manualTab.addEventListener('click', () => {
                manualTab.classList.add('active');
                imageTab.classList.remove('active');
                manu
