<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Qaimkhani Stationary & Hardware Store</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-auth-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-database-compat.js"></script>
    <style>
        :root {
            --primary: #2c3e50;
            --secondary: #3498db;
            --success: #27ae60;
            --warning: #f39c12;
            --danger: #e74c3c;
            --light: #ecf0f1;
            --dark: #2c3e50;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f8f9fa;
        }
        
        .login-container {
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            background: linear-gradient(135deg, #2c3e50 0%, #3498db 100%);
        }
        
        .login-card {
            background: white;
            border-radius: 10px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 400px;
            padding: 30px;
        }
        
        .sidebar {
            background-color: var(--primary);
            color: white;
            min-height: 100vh;
            padding: 0;
        }
        
        .sidebar .logo {
            padding: 20px;
            text-align: center;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .sidebar .nav-link {
            color: rgba(255, 255, 255, 0.8);
            padding: 15px 20px;
            border-left: 3px solid transparent;
            transition: all 0.3s;
        }
        
        .sidebar .nav-link:hover, .sidebar .nav-link.active {
            color: white;
            background-color: rgba(255, 255, 255, 0.1);
            border-left: 3px solid var(--secondary);
        }
        
        .sidebar .nav-link i {
            margin-right: 10px;
            width: 20px;
            text-align: center;
        }
        
        .main-content {
            padding: 20px;
        }
        
        .dashboard-card {
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            margin-bottom: 20px;
            border: none;
            transition: transform 0.3s;
        }
        
        .dashboard-card:hover {
            transform: translateY(-5px);
        }
        
        .card-icon {
            font-size: 2.5rem;
            opacity: 0.7;
        }
        
        .table-responsive {
            border-radius: 10px;
            overflow: hidden;
        }
        
        .btn-primary {
            background-color: var(--secondary);
            border-color: var(--secondary);
        }
        
        .btn-primary:hover {
            background-color: #2980b9;
            border-color: #2980b9;
        }
        
        .navbar {
            background-color: white;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }
        
        .page-title {
            color: var(--primary);
            margin-bottom: 20px;
            padding-bottom: 10px;
            border-bottom: 2px solid var(--light);
        }
        
        .kpi-card {
            text-align: center;
            padding: 20px;
        }
        
        .kpi-value {
            font-size: 2rem;
            font-weight: bold;
            margin: 10px 0;
        }
        
        .kpi-label {
            font-size: 0.9rem;
            color: #6c757d;
        }
        
        .form-control:focus, .form-select:focus {
            border-color: var(--secondary);
            box-shadow: 0 0 0 0.2rem rgba(52, 152, 219, 0.25);
        }
        
        .alert {
            border-radius: 10px;
            border: none;
        }
        
        .badge {
            font-size: 0.8rem;
            padding: 5px 10px;
        }
        
        .action-buttons .btn {
            margin-right: 5px;
        }
        
        .billing-item {
            border-bottom: 1px solid #eee;
            padding: 10px 0;
        }
        
        .billing-item:last-child {
            border-bottom: none;
        }
        
        .firebase-error {
            background-color: #f8d7da;
            border: 1px solid #f5c6cb;
            border-radius: 5px;
            padding: 10px;
            margin: 10px 0;
            color: #721c24;
        }
        
        @media (max-width: 768px) {
            .sidebar {
                min-height: auto;
            }
            
            .main-content {
                padding: 10px;
            }
        }
    </style>
</head>
<body>
    <!-- Login Section -->
    <div id="loginSection" class="login-container">
        <div class="login-card">
            <div class="text-center mb-4">
                <h2 class="fw-bold">Qaimkhani Store</h2>
                <p class="text-muted">Admin Login</p>
            </div>
            <form id="loginForm">
                <div class="mb-3">
                    <label for="email" class="form-label">Email address</label>
                    <input type="email" class="form-control" id="email" value="prime.can.nkt@gmail.com" required>
                </div>
                <div class="mb-3">
                    <label for="password" class="form-label">Password</label>
                    <input type="password" class="form-control" id="password" value="wassay123" required>
                </div>
                <div class="mb-3 form-check">
                    <input type="checkbox" class="form-check-input" id="rememberMe">
                    <label class="form-check-label" for="rememberMe">Remember me</label>
                </div>
                <button type="submit" class="btn btn-primary w-100">Login</button>
            </form>
            <div id="loginMessage" class="mt-3"></div>
            
            <!-- Firebase Rules Help -->
            <div class="mt-4 p-3 bg-light rounded">
                <h6><i class="fas fa-exclamation-triangle text-warning me-2"></i>Firebase Permission Issue Detected</h6>
                <p class="small mb-2">If you see "PERMISSION_DENIED" errors, you need to update your Firebase Security Rules:</p>
                <ol class="small">
                    <li>Go to <a href="https://console.firebase.google.com/" target="_blank">Firebase Console</a></li>
                    <li>Select your project "hardware-2f5af"</li>
                    <li>Go to Realtime Database → Rules</li>
                    <li>Replace the rules with:</li>
                </ol>
                <pre class="bg-dark text-white p-2 small">{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}</pre>
                <button class="btn btn-sm btn-outline-primary mt-2" onclick="showRulesHelp()">Show Detailed Instructions</button>
            </div>
        </div>
    </div>

    <!-- Firebase Rules Help Modal -->
    <div class="modal fade" id="rulesHelpModal" tabindex="-1">
        <div class="modal-dialog modal-lg">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Fix Firebase Security Rules</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
                </div>
                <div class="modal-body">
                    <h6>Step-by-Step Instructions:</h6>
                    <ol>
                        <li>Go to <a href="https://console.firebase.google.com/" target="_blank">Firebase Console</a></li>
                        <li>Select your project "hardware-2f5af"</li>
                        <li>In the left sidebar, click on "Realtime Database"</li>
                        <li>Click on the "Rules" tab</li>
                        <li>Replace the existing rules with:</li>
                    </ol>
                    <pre class="bg-dark text-white p-3">{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}</pre>
                    <p class="text-muted">These rules allow any authenticated user to read and write data.</p>
                    <div class="alert alert-warning">
                        <strong>Note:</strong> For production, you should implement more restrictive rules based on your specific requirements.
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Main Application -->
    <div id="appSection" class="d-none">
        <!-- Top Navigation -->
        <nav class="navbar navbar-expand-lg navbar-light">
            <div class="container-fluid">
                <button class="btn btn-sm" id="sidebarToggle">
                    <i class="fas fa-bars"></i>
                </button>
                <span class="navbar-brand mb-0 h1">Qaimkhani Stationary & Hardware Store</span>
                <div class="dropdown">
                    <button class="btn btn-outline-secondary dropdown-toggle" type="button" id="userDropdown" data-bs-toggle="dropdown">
                        <i class="fas fa-user-circle me-1"></i> <span id="userEmail">Admin</span>
                    </button>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="#" data-section="settings"><i class="fas fa-cog me-2"></i>Settings</a></li>
                        <li><hr class="dropdown-divider"></li>
                        <li><a class="dropdown-item" href="#" id="logoutBtn"><i class="fas fa-sign-out-alt me-2"></i>Logout</a></li>
                    </ul>
                </div>
            </div>
        </nav>

        <div class="container-fluid">
            <div class="row">
                <!-- Sidebar -->
                <div class="col-lg-2 col-md-3 sidebar" id="sidebar">
                    <div class="logo">
                        <h4><i class="fas fa-store"></i> Qaimkhani</h4>
                    </div>
                    <ul class="nav flex-column">
                        <li class="nav-item">
                            <a class="nav-link active" href="#" data-section="dashboard">
                                <i class="fas fa-tachometer-alt"></i> Dashboard
                            </a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="#" data-section="products">
                                <i class="fas fa-boxes"></i> Product Management
                            </a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="#" data-section="purchase">
                                <i class="fas fa-shopping-cart"></i> Purchase Orders
                            </a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="#" data-section="customers">
                                <i class="fas fa-users"></i> Customer Management
                            </a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="#" data-section="billing">
                                <i class="fas fa-receipt"></i> Billing & Sales
                            </a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="#" data-section="khatta">
                                <i class="fas fa-book"></i> Khatta Management
                            </a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="#" data-section="reports">
                                <i class="fas fa-chart-bar"></i> Reports
                            </a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="#" data-section="settings">
                                <i class="fas fa-cog"></i> System Settings
                            </a>
                        </li>
                    </ul>
                </div>

                <!-- Main Content -->
                <div class="col-lg-10 col-md-9 main-content" id="mainContent">
                    <!-- Firebase Error Banner -->
                    <div id="firebaseErrorBanner" class="firebase-error d-none">
                        <i class="fas fa-exclamation-triangle me-2"></i>
                        <strong>Firebase Permission Error:</strong> 
                        <span id="errorMessage">Unable to save data to Firebase. Please check your security rules.</span>
                        <button class="btn btn-sm btn-outline-danger ms-2" onclick="showRulesHelp()">Fix Rules</button>
                    </div>

                    <!-- Dashboard Section -->
                    <div id="dashboardSection" class="content-section">
                        <h2 class="page-title">Dashboard</h2>
                        
                        <!-- KPI Cards -->
                        <div class="row">
                            <div class="col-md-3">
                                <div class="card dashboard-card text-white bg-primary">
                                    <div class="card-body kpi-card">
                                        <i class="fas fa-shopping-bag card-icon"></i>
                                        <div class="kpi-value" id="totalSales">PKR 0</div>
                                        <div class="kpi-label">Total Daily Sales</div>
                                    </div>
                                </div>
                            </div>
                            <div class="col-md-3">
                                <div class="card dashboard-card text-white bg-success">
                                    <div class="card-body kpi-card">
                                        <i class="fas fa-users card-icon"></i>
                                        <div class="kpi-value" id="totalCustomers">0</div>
                                        <div class="kpi-label">Total Customers</div>
                                    </div>
                                </div>
                            </div>
                            <div class="col-md-3">
                                <div class="card dashboard-card text-white bg-warning">
                                    <div class="card-body kpi-card">
                                        <i class="fas fa-file-invoice card-icon"></i>
                                        <div class="kpi-value" id="dailyBills">0</div>
                                        <div class="kpi-label">Daily Bills</div>
                                    </div>
                                </div>
                            </div>
                            <div class="col-md-3">
                                <div class="card dashboard-card text-white bg-info">
                                    <div class="card-body kpi-card">
                                        <i class="fas fa-box card-icon"></i>
                                        <div class="kpi-value" id="lowStock">0</div>
                                        <div class="kpi-label">Low Stock Items</div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        
                        <!-- Recent Activity -->
                        <div class="row mt-4">
                            <div class="col-md-6">
                                <div class="card dashboard-card">
                                    <div class="card-header bg-light">
                                        <h5 class="mb-0"><i class="fas fa-chart-line me-2"></i>Monthly Sales Summary</h5>
                                    </div>
                                    <div class="card-body">
                                        <div id="salesChartPlaceholder" class="text-center py-4">
                                            <p class="text-muted">Sales data will appear here</p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div class="col-md-6">
                                <div class="card dashboard-card">
                                    <div class="card-header bg-light">
                                        <h5 class="mb-0"><i class="fas fa-list me-2"></i>Recent Bills</h5>
                                    </div>
                                    <div class="card-body">
                                        <div class="table-responsive">
                                            <table class="table table-hover">
                                                <thead>
                                                    <tr>
                                                        <th>Bill #</th>
                                                        <th>Customer</th>
                                                        <th>Amount</th>
                                                        <th>Date</th>
                                                    </tr>
                                                </thead>
                                                <tbody id="recentBills">
                                                    <tr>
                                                        <td colspan="4" class="text-center text-muted py-3">No recent bills</td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Product Management Section -->
                    <div id="productsSection" class="content-section d-none">
                        <h2 class="page-title">Product Management</h2>
                        
                        <div class="card dashboard-card">
                            <div class="card-header bg-light d-flex justify-content-between align-items-center">
                                <h5 class="mb-0">Product List</h5>
                                <button class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#addProductModal">
                                    <i class="fas fa-plus me-1"></i> Add Product
                                </button>
                            </div>
                            <div class="card-body">
                                <div class="row mb-3">
                                    <div class="col-md-6">
                                        <input type="text" class="form-control" id="productSearch" placeholder="Search products...">
                                    </div>
                                    <div class="col-md-3">
                                        <select class="form-select" id="categoryFilter">
                                            <option value="">All Categories</option>
                                            <option value="Stationary">Stationary</option>
                                            <option value="Hardware">Hardware</option>
                                            <option value="Electronics">Electronics</option>
                                        </select>
                                    </div>
                                    <div class="col-md-3">
                                        <button class="btn btn-outline-secondary w-100" id="clearProductFilters">Clear Filters</button>
                                    </div>
                                </div>
                                
                                <div class="table-responsive">
                                    <table class="table table-striped table-hover">
                                        <thead>
                                            <tr>
                                                <th>Product Code</th>
                                                <th>Product Name</th>
                                                <th>Category</th>
                                                <th>Price (PKR)</th>
                                                <th>Stock</th>
                                                <th>Actions</th>
                                            </tr>
                                        </thead>
                                        <tbody id="productTable">
                                            <tr>
                                                <td colspan="6" class="text-center text-muted py-3">No products added yet</td>
                                            </tr>
                                        </tbody>
                                    </table>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Purchase Order Management Section -->
                    <div id="purchaseSection" class="content-section d-none">
                        <h2 class="page-title">Purchase Order Management</h2>
                        
                        <div class="card dashboard-card">
                            <div class="card-header bg-light d-flex justify-content-between align-items-center">
                                <h5 class="mb-0">Purchase Orders</h5>
                                <button class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#addPurchaseModal">
                                    <i class="fas fa-plus me-1"></i> Add Purchase Order
                                </button>
                            </div>
                            <div class="card-body">
                                <div class="table-responsive">
                                    <table class="table table-striped table-hover">
                                        <thead>
                                            <tr>
                                                <th>Invoice #</th>
                                                <th>Supplier</th>
                                                <th>Product</th>
                                                <th>Quantity</th>
                                                <th>Price/Unit</th>
                                                <th>Total</th>
                                                <th>Date</th>
                                                <th>Actions</th>
                                            </tr>
                                        </thead>
                                        <tbody id="purchaseTable">
                                            <tr>
                                                <td colspan="8" class="text-center text-muted py-3">No purchase orders yet</td>
                                            </tr>
                                        </tbody>
                                    </table>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Customer Management Section -->
                    <div id="customersSection" class="content-section d-none">
                        <h2 class="page-title">Customer Management</h2>
                        
                        <div class="card dashboard-card">
                            <div class="card-header bg-light d-flex justify-content-between align-items-center">
                                <h5 class="mb-0">Customer List</h5>
                                <button class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#addCustomerModal">
                                    <i class="fas fa-plus me-1"></i> Add Customer
                                </button>
                            </div>
                            <div class="card-body">
                                <div class="row mb-3">
                                    <div class="col-md-6">
                                        <input type="text" class="form-control" id="customerSearch" placeholder="Search customers...">
                                    </div>
                                    <div class="col-md-3">
                                        <select class="form-select" id="statusFilter">
                                            <option value="">All Status</option>
                                            <option value="Active">Active</option>
                                            <option value="Inactive">Inactive</option>
                                        </select>
                                    </div>
                                    <div class="col-md-3">
                                        <button class="btn btn-outline-secondary w-100" id="clearCustomerFilters">Clear Filters</button>
                                    </div>
                                </div>
                                
                                <div class="table-responsive">
                                    <table class="table table-striped table-hover">
                                        <thead>
                                            <tr>
                                                <th>Customer Code</th>
                                                <th>Customer Name</th>
                                                <th>Outlet/Shop</th>
                                                <th>Phone</th>
                                                <th>Status</th>
                                                <th>Actions</th>
                                            </tr>
                                        </thead>
                                        <tbody id="customerTable">
                                            <tr>
                                                <td colspan="6" class="text-center text-muted py-3">No customers added yet</td>
                                            </tr>
                                        </tbody>
                                    </table>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Billing and Sales Section -->
                    <div id="billingSection" class="content-section d-none">
                        <h2 class="page-title">Billing & Sales Management</h2>
                        
                        <div class="card dashboard-card">
                            <div class="card-header bg-light d-flex justify-content-between align-items-center">
                                <h5 class="mb-0">Create New Bill</h5>
                                <button class="btn btn-primary" id="generateBillBtn">
                                    <i class="fas fa-file-invoice me-1"></i> Generate Bill
                                </button>
                            </div>
                            <div class="card-body">
                                <div class="row mb-3">
                                    <div class="col-md-6">
                                        <label for="billCustomer" class="form-label">Select Customer</label>
                                        <select class="form-select" id="billCustomer">
                                            <option value="">Select Customer</option>
                                        </select>
                                    </div>
                                    <div class="col-md-6">
                                        <label for="billDate" class="form-label">Bill Date</label>
                                        <input type="date" class="form-control" id="billDate">
                                    </div>
                                </div>
                                
                                <div class="row mb-3">
                                    <div class="col-md-4">
                                        <label for="billProduct" class="form-label">Select Product</label>
                                        <select class="form-select" id="billProduct">
                                            <option value="">Select Product</option>
                                        </select>
                                    </div>
                                    <div class="col-md-2">
                                        <label for="billQuantity" class="form-label">Quantity</label>
                                        <input type="number" class="form-control" id="billQuantity" min="1" value="1">
                                    </div>
                                    <div class="col-md-2">
                                        <label for="billPrice" class="form-label">Price (PKR)</label>
                                        <input type="number" class="form-control" id="billPrice" step="0.01" readonly>
                                    </div>
                                    <div class="col-md-2">
                                        <label for="billDiscount" class="form-label">Discount (PKR)</label>
                                        <input type="number" class="form-control" id="billDiscount" step="0.01" value="0">
                                    </div>
                                    <div class="col-md-2 d-flex align-items-end">
                                        <button class="btn btn-success w-100" id="addToBillBtn">
                                            <i class="fas fa-plus"></i> Add
                                        </button>
                                    </div>
                                </div>
                                
                                <div class="card mb-3">
                                    <div class="card-header">
                                        <h6 class="mb-0">Bill Items</h6>
                                    </div>
                                    <div class="card-body" id="billItemsList">
                                        <p class="text-muted text-center py-3">No items added to bill</p>
                                    </div>
                                </div>
                                
                                <div class="row">
                                    <div class="col-md-6">
                                        <div class="mb-3">
                                            <label for="paymentMethod" class="form-label">Payment Method</label>
                                            <select class="form-select" id="paymentMethod">
                                                <option value="Cash">Cash</option>
                                                <option value="Credit">Credit</option>
                                            </select>
                                        </div>
                                    </div>
                                    <div class="col-md-6">
                                        <div class="card bg-light">
                                            <div class="card-body">
                                                <div class="d-flex justify-content-between">
                                                    <span>Subtotal:</span>
                                                    <span id="billSubtotal">PKR 0.00</span>
                                                </div>
                                                <div class="d-flex justify-content-between">
                                                    <span>Discount:</span>
                                                    <span id="billTotalDiscount">PKR 0.00</span>
                                                </div>
                                                <hr>
                                                <div class="d-flex justify-content-between fw-bold">
                                                    <span>Total:</span>
                                                    <span id="billTotal">PKR 0.00</span>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        
                        <!-- Bill History -->
                        <div class="card dashboard-card mt-4">
                            <div class="card-header bg-light">
                                <h5 class="mb-0">Bill History</h5>
                            </div>
                            <div class="card-body">
                                <div class="table-responsive">
                                    <table class="table table-striped table-hover">
                                        <thead>
                                            <tr>
                                                <th>Bill #</th>
                                                <th>Customer</th>
                                                <th>Items</th>
                                                <th>Total Amount</th>
                                                <th>Payment Method</th>
                                                <th>Date</th>
                                                <th>Actions</th>
                                            </tr>
                                        </thead>
                                        <tbody id="billHistoryTable">
                                            <tr>
                                                <td colspan="7" class="text-center text-muted py-3">No bill history</td>
                                            </tr>
                                        </tbody>
                                    </table>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Khatta Management Section -->
                    <div id="khattaSection" class="content-section d-none">
                        <h2 class="page-title">Khatta (Ledger) Management</h2>
                        
                        <div class="card dashboard-card">
                            <div class="card-header bg-light d-flex justify-content-between align-items-center">
                                <h5 class="mb-0">Customer Khatta</h5>
                                <div class="d-flex">
                                    <select class="form-select me-2" id="khattaCustomerFilter">
                                        <option value="">All Customers</option>
                                    </select>
                                    <button class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#addKhattaEntryModal">
                                        <i class="fas fa-plus me-1"></i> Add Entry
                                    </button>
                                </div>
                            </div>
                            <div class="card-body">
                                <div class="table-responsive">
                                    <table class="table table-striped table-hover">
                                        <thead>
                                            <tr>
                                                <th>Date</th>
                                                <th>Customer</th>
                                                <th>Description</th>
                                                <th>Credit</th>
                                                <th>Debit</th>
                                                <th>Balance</th>
                                                <th>Actions</th>
                                            </tr>
                                        </thead>
                                        <tbody id="khattaTable">
                                            <tr>
                                                <td colspan="7" class="text-center text-muted py-3">No khatta entries yet</td>
                                            </tr>
                                        </tbody>
                                    </table>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Reports Section -->
                    <div id="reportsSection" class="content-section d-none">
                        <h2 class="page-title">Reports</h2>
                        
                        <div class="card dashboard-card">
                            <div class="card-header bg-light">
                                <h5 class="mb-0">Generate Reports</h5>
                            </div>
                            <div class="card-body">
                                <div class="row mb-4">
                                    <div class="col-md-4">
                                        <div class="card h-100">
                                            <div class="card-body text-center">
                                                <i class="fas fa-chart-bar fa-3x text-primary mb-3"></i>
                                                <h5>Sales Reports</h5>
                                                <p class="text-muted">Generate daily or monthly sales reports</p>
                                                <button class="btn btn-outline-primary" id="generateSalesReport">Generate</button>
                                            </div>
                                        </div>
                                    </div>
                                    <div class="col-md-4">
                                        <div class="card h-100">
                                            <div class="card-body text-center">
                                                <i class="fas fa-shopping-cart fa-3x text-success mb-3"></i>
                                                <h5>Purchase Reports</h5>
                                                <p class="text-muted">Generate purchase order reports</p>
                                                <button class="btn btn-outline-success" id="generatePurchaseReport">Generate</button>
                                            </div>
                                        </div>
                                    </div>
                                    <div class="col-md-4">
                                        <div class="card h-100">
                                            <div class="card-body text-center">
                                                <i class="fas fa-book fa-3x text-warning mb-3"></i>
                                                <h5>Khatta Reports</h5>
                                                <p class="text-muted">Generate customer khatta balance reports</p>
                                                <button class="btn btn-outline-warning" id="generateKhattaReport">Generate</button>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                
                                <div class="row">
                                    <div class="col-12">
                                        <div class="card">
                                            <div class="card-header">
                                                <h6 class="mb-0">Report Filters</h6>
                                            </div>
                                            <div class="card-body">
                                                <div class="row">
                                                    <div class="col-md-3">
                                                        <label for="reportType" class="form-label">Report Type</label>
                                                        <select class="form-select" id="reportType">
                                                            <option value="dailySales">Daily Sales</option>
                                                            <option value="monthlySales">Monthly Sales</option>
                                                            <option value="purchase">Purchase Orders</option>
                                                            <option value="customer">Customer List</option>
                                                            <option value="khatta">Khatta Balances</option>
                                                        </select>
                                                    </div>
                                                    <div class="col-md-3">
                                                        <label for="reportStartDate" class="form-label">Start Date</label>
                                                        <input type="date" class="form-control" id="reportStartDate">
                                                    </div>
                                                    <div class="col-md-3">
                                                        <label for="reportEndDate" class="form-label">End Date</label>
                                                        <input type="date" class="form-control" id="reportEndDate">
                                                    </div>
                                                    <div class="col-md-3">
                                                        <label for="reportCustomer" class="form-label">Customer (Optional)</label>
                                                        <select class="form-select" id="reportCustomer">
                                                            <option value="">All Customers</option>
                                                        </select>
                                                    </div>
                                                </div>
                                                <div class="mt-3">
                                                    <button class="btn btn-primary" id="generateReportBtn">
                                                        <i class="fas fa-download me-1"></i> Generate Excel Report
                                                    </button>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- System Settings Section -->
                    <div id="settingsSection" class="content-section d-none">
                        <h2 class="page-title">System Settings</h2>
                        
                        <div class="card dashboard-card">
                            <div class="card-header bg-light">
                                <h5 class="mb-0">Store Information</h5>
                            </div>
                            <div class="card-body">
                                <form id="settingsForm">
                                    <div class="row">
                                        <div class="col-md-6">
                                            <div class="mb-3">
                                                <label for="storeName" class="form-label">Store Name</label>
                                                <input type="text" class="form-control" id="storeName" value="Qaimkhani Stationary & Hardware Store">
                                            </div>
                                            <div class="mb-3">
                                                <label for="storeAddress" class="form-label">Store Address</label>
                                                <textarea class="form-control" id="storeAddress" rows="3"></textarea>
                                            </div>
                                            <div class="mb-3">
                                                <label for="storePhone" class="form-label">Phone Number</label>
                                                <input type="text" class="form-control" id="storePhone">
                                            </div>
                                        </div>
                                        <div class="col-md-6">
                                            <div class="mb-3">
                                                <label for="storeEmail" class="form-label">Email Address</label>
                                                <input type="email" class="form-control" id="storeEmail" value="wassaykk@gmail.com">
                                            </div>
                                            <div class="mb-3">
                                                <label for="currency" class="form-label">Currency</label>
                                                <select class="form-select" id="currency">
                                                    <option value="PKR" selected>Pakistani Rupee (PKR)</option>
                                                    <option value="USD">US Dollar (USD)</option>
                                                </select>
                                            </div>
                                            <div class="mb-3">
                                                <label for="dateFormat" class="form-label">Date Format</label>
                                                <select class="form-select" id="dateFormat">
                                                    <option value="dd/mm/yyyy">DD/MM/YYYY</option>
                                                    <option value="mm/dd/yyyy">MM/DD/YYYY</option>
                                                    <option value="yyyy-mm-dd">YYYY-MM-DD</option>
                                                </select>
                                            </div>
                                        </div>
                                    </div>
                                    <div class="mt-3">
                                        <button type="submit" class="btn btn-primary">Save Settings</button>
                                    </div>
                                </form>
                            </div>
                        </div>
                        
                        <div class="card dashboard-card mt-4">
                            <div class="card-header bg-light d-flex justify-content-between align-items-center">
                                <h5 class="mb-0">Database Management</h5>
                            </div>
                            <div class="card-body">
                                <div class="row">
                                    <div class="col-md-6">
                                        <div class="card h-100">
                                            <div class="card-body text-center">
                                                <i class="fas fa-database fa-3x text-primary mb-3"></i>
                                                <h5>Backup Database</h5>
                                                <p class="text-muted">Create a backup of your store data</p>
                                                <button class="btn btn-outline-primary" id="backupDbBtn">Backup Now</button>
                                            </div>
                                        </div>
                                    </div>
                                    <div class="col-md-6">
                                        <div class="card h-100">
                                            <div class="card-body text-center">
                                                <i class="fas fa-upload fa-3x text-success mb-3"></i>
                                                <h5>Restore Database</h5>
                                                <p class="text-muted">Restore data from a previous backup</p>
                                                <div class="mt-3">
                                                    <input type="file" class="form-control" id="restoreFile">
                                                    <button class="btn btn-outline-success mt-2" id="restoreDbBtn">Restore</button>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        
                        <!-- Firebase Rules Section -->
                        <div class="card dashboard-card mt-4">
                            <div class="card-header bg-light">
                                <h5 class="mb-0">Firebase Configuration</h5>
                            </div>
                            <div class="card-body">
                                <div class="alert alert-warning">
                                    <h6><i class="fas fa-exclamation-triangle me-2"></i>Firebase Permission Issue</h6>
                                    <p class="mb-2">If you're seeing "PERMISSION_DENIED" errors, you need to update your Firebase Security Rules.</p>
                                    <button class="btn btn-sm btn-outline-primary" onclick="showRulesHelp()">Show Fix Instructions</button>
                                </div>
                                
                                <div class="mt-3">
                                    <h6>Current Firebase Configuration:</h6>
                                    <div class="bg-light p-3 rounded">
                                        <pre class="small mb-0">Project: hardware-2f5af
Database: https://hardware-2f5af-default-rtdb.firebaseio.com
Status: <span id="firebaseStatus">Checking...</span></pre>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Add Product Modal -->
    <div class="modal fade" id="addProductModal" tabindex="-1">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Add New Product</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
                </div>
                <div class="modal-body">
                    <form id="addProductForm">
                        <div class="mb-3">
                            <label for="productCode" class="form-label">Product Code</label>
                            <input type="text" class="form-control" id="productCode" required>
                        </div>
                        <div class="mb-3">
                            <label for="productName" class="form-label">Product Name</label>
                            <input type="text" class="form-control" id="productName" required>
                        </div>
                        <div class="mb-3">
                            <label for="productCategory" class="form-label">Category</label>
                            <select class="form-select" id="productCategory" required>
                                <option value="">Select Category</option>
                                <option value="Stationary">Stationary</option>
                                <option value="Hardware">Hardware</option>
                                <option value="Electronics">Electronics</option>
                            </select>
                        </div>
                        <div class="mb-3">
                            <label for="productPrice" class="form-label">Price (PKR)</label>
                            <input type="number" class="form-control" id="productPrice" step="0.01" required>
                        </div>
                        <div class="mb-3">
                            <label for="productStock" class="form-label">Initial Stock</label>
                            <input type="number" class="form-control" id="productStock" required>
                        </div>
                    </form>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button>
                    <button type="button" class="btn btn-primary" id="saveProductBtn">Save Product</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Add Customer Modal -->
    <div class="modal fade" id="addCustomerModal" tabindex="-1">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Add New Customer</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
                </div>
                <div class="modal-body">
                    <form id="addCustomerForm">
                        <div class="mb-3">
                            <label for="customerCode" class="form-label">Customer Code</label>
                            <input type="text" class="form-control" id="customerCode" required>
                        </div>
                        <div class="mb-3">
                            <label for="customerName" class="form-label">Customer Name</label>
                            <input type="text" class="form-control" id="customerName" required>
                        </div>
                        <div class="mb-3">
                            <label for="customerShop" class="form-label">Outlet/Shop Name</label>
                            <input type="text" class="form-control" id="customerShop" required>
                        </div>
                        <div class="mb-3">
                            <label for="customerAddress" class="form-label">Address</label>
                            <textarea class="form-control" id="customerAddress" rows="2"></textarea>
                        </div>
                        <div class="mb-3">
                            <label for="customerPhone" class="form-label">Phone Number</label>
                            <input type="text" class="form-control" id="customerPhone" required>
                        </div>
                        <div class="mb-3">
                            <label for="customerStatus" class="form-label">Status</label>
                            <select class="form-select" id="customerStatus" required>
                                <option value="Active" selected>Active</option>
                                <option value="Inactive">Inactive</option>
                            </select>
                        </div>
                    </form>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button>
                    <button type="button" class="btn btn-primary" id="saveCustomerBtn">Save Customer</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Add Purchase Order Modal -->
    <div class="modal fade" id="addPurchaseModal" tabindex="-1">
        <div class="modal-dialog modal-lg">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Add Purchase Order</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
                </div>
                <div class="modal-body">
                    <form id="addPurchaseForm">
                        <div class="row">
                            <div class="col-md-6">
                                <div class="mb-3">
                                    <label for="purchaseInvoice" class="form-label">Invoice Number</label>
                                    <input type="text" class="form-control" id="purchaseInvoice" required>
                                </div>
                            </div>
                            <div class="col-md-6">
                                <div class="mb-3">
                                    <label for="purchaseSupplier" class="form-label">Supplier Name</label>
                                    <input type="text" class="form-control" id="purchaseSupplier" required>
                                </div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-md-6">
                                <div class="mb-3">
                                    <label for="purchaseProduct" class="form-label">Product</label>
                                    <select class="form-select" id="purchaseProduct" required>
                                        <option value="">Select Product</option>
                                    </select>
                                </div>
                            </div>
                            <div class="col-md-3">
                                <div class="mb-3">
                                    <label for="purchaseQuantity" class="form-label">Quantity</label>
                                    <input type="number" class="form-control" id="purchaseQuantity" min="1" required>
                                </div>
                            </div>
                            <div class="col-md-3">
                                <div class="mb-3">
                                    <label for="purchasePrice" class="form-label">Price per Unit (PKR)</label>
                                    <input type="number" class="form-control" id="purchasePrice" step="0.01" required>
                                </div>
                            </div>
                        </div>
                        <div class="mb-3">
                            <label for="purchaseDate" class="form-label">Purchase Date</label>
                            <input type="date" class="form-control" id="purchaseDate" required>
                        </div>
                    </form>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button>
                    <button type="button" class="btn btn-primary" id="savePurchaseBtn">Save Purchase Order</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Add Khatta Entry Modal -->
    <div class="modal fade" id="addKhattaEntryModal" tabindex="-1">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Add Khatta Entry</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
                </div>
                <div class="modal-body">
                    <form id="addKhattaForm">
                        <div class="mb-3">
                            <label for="khattaCustomer" class="form-label">Customer</label>
                            <select class="form-select" id="khattaCustomer" required>
                                <option value="">Select Customer</option>
                            </select>
                        </div>
                        <div class="mb-3">
                            <label for="khattaDate" class="form-label">Date</label>
                            <input type="date" class="form-control" id="khattaDate" required>
                        </div>
                        <div class="mb-3">
                            <label for="khattaDescription" class="form-label">Description</label>
                            <textarea class="form-control" id="khattaDescription" rows="2" required></textarea>
                        </div>
                        <div class="mb-3">
                            <label for="khattaType" class="form-label">Entry Type</label>
                            <select class="form-select" id="khattaType" required>
                                <option value="credit">Credit (Sale)</option>
                                <option value="debit">Debit (Payment)</option>
                            </select>
                        </div>
                        <div class="mb-3">
                            <label for="khattaAmount" class="form-label">Amount (PKR)</label>
                            <input type="number" class="form-control" id="khattaAmount" step="0.01" required>
                        </div>
                    </form>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button>
                    <button type="button" class="btn btn-primary" id="saveKhattaBtn">Save Entry</button>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Firebase configuration
        const firebaseConfig = {
            apiKey: "AIzaSyCwOAzbY1puo8PwnKQrmeYB5RZJFQGn1kQ",
            authDomain: "hardware-2f5af.firebaseapp.com",
            databaseURL: "https://hardware-2f5af-default-rtdb.firebaseio.com",
            projectId: "hardware-2f5af",
            storageBucket: "hardware-2f5af.firebasestorage.app",
            messagingSenderId: "73708108967",
            appId: "1:73708108967:web:f623d9ba85e049824d6f82",
            measurementId: "G-YL92FTP6VZ"
        };

        // Initialize Firebase
        firebase.initializeApp(firebaseConfig);
        const auth = firebase.auth();
        const database = firebase.database();

        // DOM elements
        const loginSection = document.getElementById('loginSection');
        const appSection = document.getElementById('appSection');
        const loginForm = document.getElementById('loginForm');
        const loginMessage = document.getElementById('loginMessage');
        const logoutBtn = document.getElementById('logoutBtn');
        const sidebarToggle = document.getElementById('sidebarToggle');
        const sidebar = document.getElementById('sidebar');
        const userEmail = document.getElementById('userEmail');
        const contentSections = document.querySelectorAll('.content-section');
        const navLinks = document.querySelectorAll('.sidebar .nav-link');
        const firebaseErrorBanner = document.getElementById('firebaseErrorBanner');
        const firebaseStatus = document.getElementById('firebaseStatus');

        // Application state
        let currentUser = null;
        let products = [];
        let customers = [];
        let purchases = [];
        let bills = [];
        let khattaEntries = [];
        let currentBillItems = [];
        let firebaseConnected = false;

        // Show Firebase rules help
        window.showRulesHelp = function() {
            const rulesModal = new bootstrap.Modal(document.getElementById('rulesHelpModal'));
            rulesModal.show();
        };

        // Check Firebase connection
        function checkFirebaseConnection() {
            database.ref('.info/connected').on('value', function(snap) {
                if (snap.val() === true) {
                    firebaseConnected = true;
                    firebaseStatus.textContent = 'Connected';
                    firebaseStatus.className = 'text-success';
                    firebaseErrorBanner.classList.add('d-none');
                } else {
                    firebaseConnected = false;
                    firebaseStatus.textContent = 'Disconnected';
                    firebaseStatus.className = 'text-danger';
                }
            });
            
            // Test write permissions
            testFirebasePermissions();
        }

        // Test Firebase write permissions
        function testFirebasePermissions() {
            const testRef = database.ref('permission_test');
            testRef.set({test: Date.now()})
                .then(() => {
                    firebaseConnected = true;
                    firebaseStatus.textContent = 'Connected (Write permissions OK)';
                    firebaseStatus.className = 'text-success';
                    firebaseErrorBanner.classList.add('d-none');
                    // Clean up test data
                    testRef.remove();
                })
                .catch((error) => {
                    if (error.code === 'PERMISSION_DENIED') {
                        firebaseStatus.textContent = 'Connected (No write permissions)';
                        firebaseStatus.className = 'text-warning';
                        showFirebaseError('Firebase permission denied. You need to update security rules to allow writes.');
                    } else {
                        firebaseStatus.textContent = 'Error: ' + error.message;
                        firebaseStatus.className = 'text-danger';
                    }
                });
        }

        // Show Firebase error
        function showFirebaseError(message) {
            document.getElementById('errorMessage').textContent = message;
            firebaseErrorBanner.classList.remove('d-none');
        }

        // Hide Firebase error
        function hideFirebaseError() {
            firebaseErrorBanner.classList.add('d-none');
        }

        // Initialize the application
        document.addEventListener('DOMContentLoaded', function() {
            // Set current date for date inputs
            const today = new Date().toISOString().split('T')[0];
            document.getElementById('billDate').value = today;
            document.getElementById('purchaseDate').value = today;
            document.getElementById('khattaDate').value = today;
            document.getElementById('reportStartDate').value = today;
            document.getElementById('reportEndDate').value = today;
            
            // Check Firebase connection
            checkFirebaseConnection();
            
            // Check if user is already logged in
            auth.onAuthStateChanged((user) => {
                if (user) {
                    // User is signed in
                    currentUser = user;
                    userEmail.textContent = user.email;
                    loginSection.classList.add('d-none');
                    appSection.classList.remove('d-none');
                    loadInitialData();
                } else {
                    // User is signed out
                    loginSection.classList.remove('d-none');
                    appSection.classList.add('d-none');
                }
            });
        });

        // Login functionality
        loginForm.addEventListener('submit', (e) => {
            e.preventDefault();
            
            const email = document.getElementById('email').value;
            const password = document.getElementById('password').value;
            
            loginMessage.innerHTML = '<div class="alert alert-info">Logging in...</div>';
            
            auth.signInWithEmailAndPassword(email, password)
                .then((userCredential) => {
                    // Login successful
                    loginMessage.innerHTML = '<div class="alert alert-success">Login successful! Redirecting...</div>';
                    
                    // Store user info
                    currentUser = userCredential.user;
                    userEmail.textContent = currentUser.email;
                    
                    // Switch to app view
                    setTimeout(() => {
                        loginSection.classList.add('d-none');
                        appSection.classList.remove('d-none');
                        loadInitialData();
                    }, 1000);
                })
                .catch((error) => {
                    // Login failed
                    let errorMessage = "Login failed. Please try again.";
                    
                    if (error.code === 'auth/user-not-found') {
                        errorMessage = "No account found with this email.";
                    } else if (error.code === 'auth/wrong-password') {
                        errorMessage = "Incorrect password.";
                    }
                    
                    loginMessage.innerHTML = `<div class="alert alert-danger">${errorMessage}</div>`;
                });
        });

        // Logout functionality
        logoutBtn.addEventListener('click', () => {
            auth.signOut().then(() => {
                appSection.classList.add('d-none');
                loginSection.classList.remove('d-none');
                loginMessage.innerHTML = '<div class="alert alert-info">You have been logged out.</div>';
            });
        });

        // Sidebar toggle for mobile
        sidebarToggle.addEventListener('click', () => {
            sidebar.classList.toggle('d-none');
        });

        // Navigation between sections
        navLinks.forEach(link => {
            link.addEventListener('click', (e) => {
                e.preventDefault();
                
                // Remove active class from all links
                navLinks.forEach(l => l.classList.remove('active'));
                
                // Add active class to clicked link
                link.classList.add('active');
                
                // Hide all sections
                contentSections.forEach(section => {
                    section.classList.add('d-none');
                });
                
                // Show selected section
                const sectionId = link.getAttribute('data-section') + 'Section';
                document.getElementById(sectionId).classList.remove('d-none');
                
                // Load section-specific data
                if (sectionId === 'dashboardSection') {
                    loadDashboardData();
                } else if (sectionId === 'productsSection') {
                    loadProducts();
                } else if (sectionId === 'customersSection') {
                    loadCustomers();
                } else if (sectionId === 'purchaseSection') {
                    loadPurchases();
                } else if (sectionId === 'billingSection') {
                    loadBillingData();
                } else if (sectionId === 'khattaSection') {
                    loadKhattaData();
                } else if (sectionId === 'reportsSection') {
                    loadReportsData();
                } else if (sectionId === 'settingsSection') {
                    loadSettings();
                }
            });
        });

        // Load initial data from Firebase
        function loadInitialData() {
            // Load products
            database.ref('products').on('value', (snapshot) => {
                products = snapshot.val() || [];
                updateProductSelects();
                if (document.getElementById('productsSection').classList.contains('d-none') === false) {
                    loadProducts();
                }
            }, (error) => {
                if (error.code === 'PERMISSION_DENIED') {
                    showFirebaseError('Cannot load products: ' + error.message);
                }
            });
            
            // Load customers
            database.ref('customers').on('value', (snapshot) => {
                customers = snapshot.val() || [];
                updateCustomerSelects();
                if (document.getElementById('customersSection').classList.contains('d-none') === false) {
                    loadCustomers();
                }
            }, (error) => {
                if (error.code === 'PERMISSION_DENIED') {
                    showFirebaseError('Cannot load customers: ' + error.message);
                }
            });
            
            // Load purchases
            database.ref('purchases').on('value', (snapshot) => {
                purchases = snapshot.val() || [];
                if (document.getElementById('purchaseSection').classList.contains('d-none') === false) {
                    loadPurchases();
                }
            }, (error) => {
                if (error.code === 'PERMISSION_DENIED') {
                    showFirebaseError('Cannot load purchases: ' + error.message);
                }
            });
            
            // Load bills
            database.ref('bills').on('value', (snapshot) => {
                bills = snapshot.val() || [];
                if (document.getElementById('billingSection').classList.contains('d-none') === false) {
                    loadBillingData();
                }
                if (document.getElementById('dashboardSection').classList.contains('d-none') === false) {
                    loadDashboardData();
                }
            }, (error) => {
                if (error.code === 'PERMISSION_DENIED') {
                    showFirebaseError('Cannot load bills: ' + error.message);
                }
            });
            
            // Load khatta entries
            database.ref('khatta').on('value', (snapshot) => {
                khattaEntries = snapshot.val() || [];
                if (document.getElementById('khattaSection').classList.contains('d-none') === false) {
                    loadKhattaData();
                }
            }, (error) => {
                if (error.code === 'PERMISSION_DENIED') {
                    showFirebaseError('Cannot load khatta entries: ' + error.message);
                }
            });
            
            // Load settings
            database.ref('settings').once('value').then((snapshot) => {
                const settings = snapshot.val() || {};
                if (settings.storeName) {
                    document.getElementById('storeName').value = settings.storeName;
                }
                if (settings.storeAddress) {
                    document.getElementById('storeAddress').value = settings.storeAddress;
                }
                if (settings.storePhone) {
                    document.getElementById('storePhone').value = settings.storePhone;
                }
                if (settings.storeEmail) {
                    document.getElementById('storeEmail').value = settings.storeEmail;
                }
                if (settings.currency) {
                    document.getElementById('currency').value = settings.currency;
                }
                if (settings.dateFormat) {
                    document.getElementById('dateFormat').value = settings.dateFormat;
                }
            }).catch((error) => {
                if (error.code === 'PERMISSION_DENIED') {
                    showFirebaseError('Cannot load settings: ' + error.message);
                }
            });
            
            // Load dashboard data
            loadDashboardData();
        }

        // Update product dropdowns
        function updateProductSelects() {
            const productSelects = [
                document.getElementById('billProduct'),
                document.getElementById('purchaseProduct')
            ];
            
            productSelects.forEach(select => {
                if (select) {
                    // Clear existing options except the first one
                    while (select.options.length > 1) {
                        select.remove(1);
                    }
                    
                    // Add products to dropdown
                    if (Array.isArray(products)) {
                        products.forEach((product, index) => {
                            if (product && product.name) {
                                const option = document.createElement('option');
                                option.value = index;
                                option.textContent = `${product.name} (${product.code}) - Stock: ${product.stock || 0}`;
                                select.appendChild(option);
                            }
                        });
                    }
                }
            });
            
            // Update product price when selected in billing
            const billProductSelect = document.getElementById('billProduct');
            if (billProductSelect) {
                billProductSelect.addEventListener('change', function() {
                    const selectedIndex = this.value;
                    if (selectedIndex && products[selectedIndex]) {
                        document.getElementById('billPrice').value = products[selectedIndex].price || 0;
                    } else {
                        document.getElementById('billPrice').value = '';
                    }
                });
            }
        }

        // Update customer dropdowns
        function updateCustomerSelects() {
            const customerSelects = [
                document.getElementById('billCustomer'),
                document.getElementById('khattaCustomer'),
                document.getElementById('khattaCustomerFilter'),
                document.getElementById('reportCustomer')
            ];
            
            customerSelects.forEach(select => {
                if (select) {
                    // Clear existing options except the first one
                    while (select.options.length > 1) {
                        select.remove(1);
                    }
                    
                    // Add customers to dropdown
                    if (Array.isArray(customers)) {
                        customers.forEach((customer, index) => {
                            if (customer && customer.name) {
                                const option = document.createElement('option');
                                option.value = index;
                                option.textContent = `${customer.name} (${customer.shop})`;
                                select.appendChild(option);
                            }
                        });
                    }
                }
            });
        }

        // Load dashboard data
        function loadDashboardData() {
            // Calculate KPIs
            const today = new Date().toISOString().split('T')[0];
            let dailySales = 0;
            let dailyBillsCount = 0;
            let lowStockCount = 0;
            
            // Calculate daily sales and bills
            if (Array.isArray(bills)) {
                bills.forEach(bill => {
                    if (bill.date === today) {
                        dailySales += parseFloat(bill.total || 0);
                        dailyBillsCount++;
                    }
                });
            }
            
            // Calculate low stock items
            if (Array.isArray(products)) {
                products.forEach(product => {
                    if (product && product.stock && product.stock < 10) {
                        lowStockCount++;
                    }
                });
            }
            
            // Update KPI cards
            document.getElementById('totalSales').textContent = `PKR ${dailySales.toFixed(2)}`;
            document.getElementById('totalCustomers').textContent = Array.isArray(customers) ? customers.length : 0;
            document.getElementById('dailyBills').textContent = dailyBillsCount;
            document.getElementById('lowStock').textContent = lowStockCount;
            
            // Update recent bills table
            const recentBillsTable = document.getElementById('recentBills');
            recentBillsTable.innerHTML = '';
            
            if (Array.isArray(bills) && bills.length > 0) {
                // Sort bills by date (newest first) and take latest 5
                const sortedBills = [...bills].sort((a, b) => new Date(b.date) - new Date(a.date)).slice(0, 5);
                
                sortedBills.forEach(bill => {
                    const row = document.createElement('tr');
                    row.innerHTML = `
                        <td>${bill.id || 'N/A'}</td>
                        <td>${bill.customerName || 'N/A'}</td>
                        <td>PKR ${parseFloat(bill.total || 0).toFixed(2)}</td>
                        <td>${bill.date || 'N/A'}</td>
                    `;
                    recentBillsTable.appendChild(row);
                });
            } else {
                recentBillsTable.innerHTML = `
                    <tr>
                        <td colspan="4" class="text-center text-muted py-3">No recent bills</td>
                    </tr>
                `;
            }
        }

        // Load products
        function loadProducts() {
            const productTable = document.getElementById('productTable');
            productTable.innerHTML = '';
            
            if (Array.isArray(products) && products.length > 0) {
                products.forEach((product, index) => {
                    if (product) {
                        const row = document.createElement('tr');
                        row.innerHTML = `
                            <td>${product.code || 'N/A'}</td>
                            <td>${product.name || 'N/A'}</td>
                            <td>${product.category || 'N/A'}</td>
                            <td>PKR ${parseFloat(product.price || 0).toFixed(2)}</td>
                            <td>${product.stock || 0}</td>
                            <td class="action-buttons">
                                <button class="btn btn-sm btn-warning" onclick="editProduct(${index})">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="btn btn-sm btn-danger" onclick="deleteProduct(${index})">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </td>
                        `;
                        productTable.appendChild(row);
                    }
                });
            } else {
                productTable.innerHTML = `
                    <tr>
                        <td colspan="6" class="text-center text-muted py-3">No products added yet</td>
                    </tr>
                `;
            }
        }

        // Load customers
        function loadCustomers() {
            const customerTable = document.getElementById('customerTable');
            customerTable.innerHTML = '';
            
            if (Array.isArray(customers) && customers.length > 0) {
                customers.forEach((customer, index) => {
                    if (customer) {
                        const statusBadge = customer.status === 'Active' ? 
                            '<span class="badge bg-success">Active</span>' : 
                            '<span class="badge bg-secondary">Inactive</span>';
                        
                        const row = document.createElement('tr');
                        row.innerHTML = `
                            <td>${customer.code || 'N/A'}</td>
                            <td>${customer.name || 'N/A'}</td>
                            <td>${customer.shop || 'N/A'}</td>
                            <td>${customer.phone || 'N/A'}</td>
                            <td>${statusBadge}</td>
                            <td class="action-buttons">
                                <button class="btn btn-sm btn-warning" onclick="editCustomer(${index})">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="btn btn-sm btn-danger" onclick="deleteCustomer(${index})">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </td>
                        `;
                        customerTable.appendChild(row);
                    }
                });
            } else {
                customerTable.innerHTML = `
                    <tr>
                        <td colspan="6" class="text-center text-muted py-3">No customers added yet</td>
                    </tr>
                `;
            }
        }

        // Load purchases
        function loadPurchases() {
            const purchaseTable = document.getElementById('purchaseTable');
            purchaseTable.innerHTML = '';
            
            if (Array.isArray(purchases) && purchases.length > 0) {
                purchases.forEach((purchase, index) => {
                    if (purchase) {
                        const product = products[purchase.productIndex];
                        const productName = product ? product.name : 'N/A';
                        const total = parseFloat(purchase.quantity || 0) * parseFloat(purchase.price || 0);
                        
                        const row = document.createElement('tr');
                        row.innerHTML = `
                            <td>${purchase.invoice || 'N/A'}</td>
                            <td>${purchase.supplier || 'N/A'}</td>
                            <td>${productName}</td>
                            <td>${purchase.quantity || 0}</td>
                            <td>PKR ${parseFloat(purchase.price || 0).toFixed(2)}</td>
                            <td>PKR ${total.toFixed(2)}</td>
                            <td>${purchase.date || 'N/A'}</td>
                            <td class="action-buttons">
                                <button class="btn btn-sm btn-danger" onclick="deletePurchase(${index})">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </td>
                        `;
                        purchaseTable.appendChild(row);
                    }
                });
            } else {
                purchaseTable.innerHTML = `
                    <tr>
                        <td colspan="8" class="text-center text-muted py-3">No purchase orders yet</td>
                    </tr>
                `;
            }
        }

        // Load billing data
        function loadBillingData() {
            // Reset current bill
            currentBillItems = [];
            updateBillSummary();
            
            // Load bill history
            const billHistoryTable = document.getElementById('billHistoryTable');
            billHistoryTable.innerHTML = '';
            
            if (Array.isArray(bills) && bills.length > 0) {
                bills.forEach((bill, index) => {
                    if (bill) {
                        const paymentBadge = bill.paymentMethod === 'Cash' ? 
                            '<span class="badge bg-success">Cash</span>' : 
                            '<span class="badge bg-warning">Credit</span>';
                        
                        const row = document.createElement('tr');
                        row.innerHTML = `
                            <td>${bill.id || 'N/A'}</td>
                            <td>${bill.customerName || 'N/A'}</td>
                            <td>${bill.items ? bill.items.length : 0} items</td>
                            <td>PKR ${parseFloat(bill.total || 0).toFixed(2)}</td>
                            <td>${paymentBadge}</td>
                            <td>${bill.date || 'N/A'}</td>
                            <td class="action-buttons">
                                <button class="btn btn-sm btn-info" onclick="viewBill(${index})">
                                    <i class="fas fa-eye"></i>
                                </button>
                                <button class="btn btn-sm btn-danger" onclick="deleteBill(${index})">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </td>
                        `;
                        billHistoryTable.appendChild(row);
                    }
                });
            } else {
                billHistoryTable.innerHTML = `
                    <tr>
                        <td colspan="7" class="text-center text-muted py-3">No bill history</td>
                    </tr>
                `;
            }
        }

        // Load khatta data
        function loadKhattaData() {
            const khattaTable = document.getElementById('khattaTable');
            khattaTable.innerHTML = '';
            
            // Filter khatta entries if a customer is selected
            const customerFilter = document.getElementById('khattaCustomerFilter').value;
            let filteredEntries = khattaEntries;
            
            if (customerFilter !== '' && Array.isArray(khattaEntries)) {
                filteredEntries = khattaEntries.filter(entry => 
                    entry && entry.customerIndex == customerFilter
                );
            }
            
            if (Array.isArray(filteredEntries) && filteredEntries.length > 0) {
                // Calculate running balance for each customer
                const customerBalances = {};
                
                filteredEntries.forEach((entry, index) => {
                    if (entry) {
                        const customer = customers[entry.customerIndex];
                        const customerName = customer ? `${customer.name} (${customer.shop})` : 'N/A';
                        
                        if (!customerBalances[entry.customerIndex]) {
                            customerBalances[entry.customerIndex] = 0;
                        }
                        
                        if (entry.type === 'credit') {
                            customerBalances[entry.customerIndex] += parseFloat(entry.amount || 0);
                        } else {
                            customerBalances[entry.customerIndex] -= parseFloat(entry.amount || 0);
                        }
                        
                        const creditAmount = entry.type === 'credit' ? parseFloat(entry.amount || 0).toFixed(2) : '0.00';
                        const debitAmount = entry.type === 'debit' ? parseFloat(entry.amount || 0).toFixed(2) : '0.00';
                        
                        const row = document.createElement('tr');
                        row.innerHTML = `
                            <td>${entry.date || 'N/A'}</td>
                            <td>${customerName}</td>
                            <td>${entry.description || 'N/A'}</td>
                            <td>PKR ${creditAmount}</td>
                            <td>PKR ${debitAmount}</td>
                            <td>PKR ${customerBalances[entry.customerIndex].toFixed(2)}</td>
                            <td class="action-buttons">
                                <button class="btn btn-sm btn-danger" onclick="deleteKhattaEntry(${index})">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </td>
                        `;
                        khattaTable.appendChild(row);
                    }
                });
            } else {
                khattaTable.innerHTML = `
                    <tr>
                        <td colspan="7" class="text-center text-muted py-3">No khatta entries yet</td>
                    </tr>
                `;
            }
        }

        // Load reports data
        function loadReportsData() {
            // This function would prepare data for reports
            // In a full implementation, this would set up filters and previews
        }

        // Load settings
        function loadSettings() {
            // Settings are loaded in loadInitialData
        }

        // Add product functionality
        document.getElementById('saveProductBtn').addEventListener('click', function() {
            const productCode = document.getElementById('productCode').value;
            const productName = document.getElementById('productName').value;
            const productCategory = document.getElementById('productCategory').value;
            const productPrice = parseFloat(document.getElementById('productPrice').value);
            const productStock = parseInt(document.getElementById('productStock').value);
            
            if (!productCode || !productName || !productCategory || isNaN(productPrice) || isNaN(productStock)) {
                alert('Please fill in all fields with valid values');
                return;
            }
            
            const newProduct = {
                code: productCode,
                name: productName,
                category: productCategory,
                price: productPrice,
                stock: productStock
            };
            
            // Add to Firebase
            if (!Array.isArray(products)) {
                products = [];
            }
            products.push(newProduct);
            database.ref('products').set(products)
                .then(() => {
                    alert('Product added successfully!');
                    $('#addProductModal').modal('hide');
                    document.getElementById('addProductForm').reset();
                    loadProducts();
                    hideFirebaseError();
                })
                .catch(error => {
                    if (error.code === 'PERMISSION_DENIED') {
                        showFirebaseError('Cannot save product: ' + error.message);
                        alert('Error: Cannot save product due to Firebase permissions. Please check security rules.');
                    } else {
                        alert('Error adding product: ' + error.message);
                    }
                });
        });

        // Add customer functionality
        document.getElementById('saveCustomerBtn').addEventListener('click', function() {
            const customerCode = document.getElementById('customerCode').value;
            const customerName = document.getElementById('customerName').value;
            const customerShop = document.getElementById('customerShop').value;
            const customerAddress = document.getElementById('customerAddress').value;
            const customerPhone = document.getElementById('customerPhone').value;
            const customerStatus = document.getElementById('customerStatus').value;
            
            if (!customerCode || !customerName || !customerShop || !customerPhone) {
                alert('Please fill in all required fields');
                return;
            }
            
            const newCustomer = {
                code: customerCode,
                name: customerName,
                shop: customerShop,
                address: customerAddress,
                phone: customerPhone,
                status: customerStatus
            };
            
            // Add to Firebase
            if (!Array.isArray(customers)) {
                customers = [];
            }
            customers.push(newCustomer);
            database.ref('customers').set(customers)
                .then(() => {
                    alert('Customer added successfully!');
                    $('#addCustomerModal').modal('hide');
                    document.getElementById('addCustomerForm').reset();
                    loadCustomers();
                    hideFirebaseError();
                })
                .catch(error => {
                    if (error.code === 'PERMISSION_DENIED') {
                        showFirebaseError('Cannot save customer: ' + error.message);
                        alert('Error: Cannot save customer due to Firebase permissions. Please check security rules.');
                    } else {
                        alert('Error adding customer: ' + error.message);
                    }
                });
        });

        // Add purchase order functionality
        document.getElementById('savePurchaseBtn').addEventListener('click', function() {
            const purchaseInvoice = document.getElementById('purchaseInvoice').value;
            const purchaseSupplier = document.getElementById('purchaseSupplier').value;
            const purchaseProductIndex = document.getElementById('purchaseProduct').value;
            const purchaseQuantity = parseInt(document.getElementById('purchaseQuantity').value);
            const purchasePrice = parseFloat(document.getElementById('purchasePrice').value);
            const purchaseDate = document.getElementById('purchaseDate').value;
            
            if (!purchaseInvoice || !purchaseSupplier || !purchaseProductIndex || 
                isNaN(purchaseQuantity) || isNaN(purchasePrice) || !purchaseDate) {
                alert('Please fill in all fields with valid values');
                return;
            }
            
            const newPurchase = {
                invoice: purchaseInvoice,
                supplier: purchaseSupplier,
                productIndex: parseInt(purchaseProductIndex),
                quantity: purchaseQuantity,
                price: purchasePrice,
                date: purchaseDate
            };
            
            // Update product stock
            if (products[purchaseProductIndex]) {
                products[purchaseProductIndex].stock = 
                    (parseInt(products[purchaseProductIndex].stock) || 0) + purchaseQuantity;
            }
            
            // Add to Firebase
            if (!Array.isArray(purchases)) {
                purchases = [];
            }
            purchases.push(newPurchase);
            
            // Update both purchases and products in Firebase
            database.ref('purchases').set(purchases)
                .then(() => {
                    return database.ref('products').set(products);
                })
                .then(() => {
                    alert('Purchase order added successfully!');
                    $('#addPurchaseModal').modal('hide');
                    document.getElementById('addPurchaseForm').reset();
                    loadPurchases();
                    hideFirebaseError();
                })
                .catch(error => {
                    if (error.code === 'PERMISSION_DENIED') {
                        showFirebaseError('Cannot save purchase: ' + error.message);
                        alert('Error: Cannot save purchase due to Firebase permissions. Please check security rules.');
                    } else {
                        alert('Error adding purchase order: ' + error.message);
                    }
                });
        });

        // Add item to bill
        document.getElementById('addToBillBtn').addEventListener('click', function() {
            const productIndex = document.getElementById('billProduct').value;
            const quantity = parseInt(document.getElementById('billQuantity').value);
            const price = parseFloat(document.getElementById('billPrice').value);
            const discount = parseFloat(document.getElementById('billDiscount').value);
            
            if (!productIndex || isNaN(quantity) || quantity < 1 || isNaN(price)) {
                alert('Please select a product and enter valid quantity');
                return;
            }
            
            const product = products[productIndex];
            if (!product) {
                alert('Selected product not found');
                return;
            }
            
            if (quantity > (product.stock || 0)) {
                alert('Insufficient stock available');
                return;
            }
            
            const itemTotal = (price * quantity) - discount;
            const newItem = {
                productIndex: parseInt(productIndex),
                productName: product.name,
                quantity: quantity,
                price: price,
                discount: discount,
                total: itemTotal
            };
            
            currentBillItems.push(newItem);
            updateBillItemsList();
            updateBillSummary();
            
            // Reset form
            document.getElementById('billQuantity').value = 1;
            document.getElementById('billDiscount').value = 0;
        });

        // Update bill items list
        function updateBillItemsList() {
            const billItemsList = document.getElementById('billItemsList');
            billItemsList.innerHTML = '';
            
            if (currentBillItems.length === 0) {
                billItemsList.innerHTML = '<p class="text-muted text-center py-3">No items added to bill</p>';
                return;
            }
            
            currentBillItems.forEach((item, index) => {
                const itemDiv = document.createElement('div');
                itemDiv.className = 'billing-item';
                itemDiv.innerHTML = `
                    <div class="d-flex justify-content-between align-items-center">
                        <div>
                            <strong>${item.productName}</strong><br>
                            <small>Qty: ${item.quantity} × PKR ${item.price.toFixed(2)}</small>
                            ${item.discount > 0 ? `<br><small>Discount: PKR ${item.discount.toFixed(2)}</small>` : ''}
                        </div>
                        <div class="text-end">
                            <strong>PKR ${item.total.toFixed(2)}</strong><br>
                            <button class="btn btn-sm btn-danger mt-1" onclick="removeBillItem(${index})">
                                <i class="fas fa-times"></i>
                            </button>
                        </div>
                    </div>
                `;
                billItemsList.appendChild(itemDiv);
            });
        }

        // Remove item from bill
        window.removeBillItem = function(index) {
            currentBillItems.splice(index, 1);
            updateBillItemsList();
            updateBillSummary();
        };

        // Update bill summary
        function updateBillSummary() {
            const subtotal = currentBillItems.reduce((sum, item) => sum + (item.price * item.quantity), 0);
            const totalDiscount = currentBillItems.reduce((sum, item) => sum + item.discount, 0);
            const total = subtotal - totalDiscount;
            
            document.getElementById('billSubtotal').textContent = `PKR ${subtotal.toFixed(2)}`;
            document.getElementById('billTotalDiscount').textContent = `PKR ${totalDiscount.toFixed(2)}`;
            document.getElementById('billTotal').textContent = `PKR ${total.toFixed(2)}`;
        }

        // Generate bill
        document.getElementById('generateBillBtn').addEventListener('click', function() {
            const customerIndex = document.getElementById('billCustomer').value;
            const paymentMethod = document.getElementById('paymentMethod').value;
            const billDate = document.getElementById('billDate').value;
            
            if (!customerIndex) {
                alert('Please select a customer');
                return;
            }
            
            if (currentBillItems.length === 0) {
                alert('Please add at least one item to the bill');
                return;
            }
            
            const customer = customers[customerIndex];
            if (!customer) {
                alert('Selected customer not found');
                return;
            }
            
            const subtotal = currentBillItems.reduce((sum, item) => sum + (item.price * item.quantity), 0);
            const totalDiscount = currentBillItems.reduce((sum, item) => sum + item.discount, 0);
            const total = subtotal - totalDiscount;
            
            // Generate bill ID
            const billId = 'B-' + new Date().getTime().toString().slice(-6);
            
            const newBill = {
                id: billId,
                customerIndex: parseInt(customerIndex),
                customerName: customer.name,
                customerShop: customer.shop,
                items: [...currentBillItems],
                subtotal: subtotal,
                discount: totalDiscount,
                total: total,
                paymentMethod: paymentMethod,
                date: billDate
            };
            
            // Update product stocks
            currentBillItems.forEach(item => {
                if (products[item.productIndex]) {
                    products[item.productIndex].stock = 
                        (parseInt(products[item.productIndex].stock) || 0) - item.quantity;
                }
            });
            
            // Add khatta entry if payment is credit
            if (paymentMethod === 'Credit') {
                const newKhattaEntry = {
                    customerIndex: parseInt(customerIndex),
                    date: billDate,
                    description: `Sale - Bill ${billId}`,
                    type: 'credit',
                    amount: total
                };
                
                if (!Array.isArray(khattaEntries)) {
                    khattaEntries = [];
                }
                khattaEntries.push(newKhattaEntry);
            }
            
            // Add to Firebase
            if (!Array.isArray(bills)) {
                bills = [];
            }
            bills.push(newBill);
            
            // Update multiple Firebase references
            const updates = {
                '/bills': bills,
                '/products': products
            };
            
            if (paymentMethod === 'Credit') {
                updates['/khatta'] = khattaEntries;
            }
            
            database.ref().update(updates)
                .then(() => {
                    alert(`Bill ${billId} generated successfully!`);
                    loadBillingData();
                    loadDashboardData();
                    hideFirebaseError();
                })
                .catch(error => {
                    if (error.code === 'PERMISSION_DENIED') {
                        showFirebaseError('Cannot save bill: ' + error.message);
                        alert('Error: Cannot save bill due to Firebase permissions. Please check security rules.');
                    } else {
                        alert('Error generating bill: ' + error.message);
                    }
                });
        });

        // Add khatta entry
        document.getElementById('saveKhattaBtn').addEventListener('click', function() {
            const customerIndex = document.getElementById('khattaCustomer').value;
            const khattaDate = document.getElementById('khattaDate').value;
            const khattaDescription = document.getElementById('khattaDescription').value;
            const khattaType = document.getElementById('khattaType').value;
            const khattaAmount = parseFloat(document.getElementById('khattaAmount').value);
            
            if (!customerIndex || !khattaDate || !khattaDescription || !khattaType || isNaN(khattaAmount)) {
                alert('Please fill in all fields with valid values');
                return;
            }
            
            const newKhattaEntry = {
                customerIndex: parseInt(customerIndex),
                date: khattaDate,
                description: khattaDescription,
                type: khattaType,
                amount: khattaAmount
            };
            
            // Add to Firebase
            if (!Array.isArray(khattaEntries)) {
                khattaEntries = [];
            }
            khattaEntries.push(newKhattaEntry);
            database.ref('khatta').set(khattaEntries)
                .then(() => {
                    alert('Khatta entry added successfully!');
                    $('#addKhattaEntryModal').modal('hide');
                    document.getElementById('addKhattaForm').reset();
                    loadKhattaData();
                    hideFirebaseError();
                })
                .catch(error => {
                    if (error.code === 'PERMISSION_DENIED') {
                        showFirebaseError('Cannot save khatta entry: ' + error.message);
                        alert('Error: Cannot save khatta entry due to Firebase permissions. Please check security rules.');
                    } else {
                        alert('Error adding khatta entry: ' + error.message);
                    }
                });
        });

        // Save settings
        document.getElementById('settingsForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const settings = {
                storeName: document.getElementById('storeName').value,
                storeAddress: document.getElementById('storeAddress').value,
                storePhone: document.getElementById('storePhone').value,
                storeEmail: document.getElementById('storeEmail').value,
                currency: document.getElementById('currency').value,
                dateFormat: document.getElementById('dateFormat').value
            };
            
            database.ref('settings').set(settings)
                .then(() => {
                    alert('Settings saved successfully!');
                    hideFirebaseError();
                })
                .catch(error => {
                    if (error.code === 'PERMISSION_DENIED') {
                        showFirebaseError('Cannot save settings: ' + error.message);
                        alert('Error saving settings: PERMISSION_DENIED: Permission denied. Please check Firebase security rules.');
                    } else {
                        alert('Error saving settings: ' + error.message);
                    }
                });
        });

        // Generate reports
        document.getElementById('generateReportBtn').addEventListener('click', function() {
            const reportType = document.getElementById('reportType').value;
            const startDate = document.getElementById('reportStartDate').value;
            const endDate = document.getElementById('reportEndDate').value;
            const customerIndex = document.getElementById('reportCustomer').value;
            
            if (!startDate || !endDate) {
                alert('Please select start and end dates');
                return;
            }
            
            // In a full implementation, this would generate an Excel file
            // For this demo, we'll just show an alert
            alert(`Report generated for ${reportType} from ${startDate} to ${endDate}`);
        });

        // Filter products
        document.getElementById('productSearch').addEventListener('input', function() {
            filterProducts();
        });

        document.getElementById('categoryFilter').addEventListener('change', function() {
            filterProducts();
        });

        document.getElementById('clearProductFilters').addEventListener('click', function() {
            document.getElementById('productSearch').value = '';
            document.getElementById('categoryFilter').value = '';
            filterProducts();
        });

        function filterProducts() {
            const searchTerm = document.getElementById('productSearch').value.toLowerCase();
            const categoryFilter = document.getElementById('categoryFilter').value;
            
            const productTable = document.getElementById('productTable');
            productTable.innerHTML = '';
            
            let filteredProducts = products;
            
            if (searchTerm) {
                filteredProducts = filteredProducts.filter(product => 
                    product && (
                        (product.code && product.code.toLowerCase().includes(searchTerm)) ||
                        (product.name && product.name.toLowerCase().includes(searchTerm))
                    )
                );
            }
            
            if (categoryFilter) {
                filteredProducts = filteredProducts.filter(product => 
                    product && product.category === categoryFilter
                );
            }
            
            if (filteredProducts.length > 0) {
                filteredProducts.forEach((product, index) => {
                    if (product) {
                        const row = document.createElement('tr');
                        row.innerHTML = `
                            <td>${product.code || 'N/A'}</td>
                            <td>${product.name || 'N/A'}</td>
                            <td>${product.category || 'N/A'}</td>
                            <td>PKR ${parseFloat(product.price || 0).toFixed(2)}</td>
                            <td>${product.stock || 0}</td>
                            <td class="action-buttons">
                                <button class="btn btn-sm btn-warning" onclick="editProduct(${products.indexOf(product)})">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="btn btn-sm btn-danger" onclick="deleteProduct(${products.indexOf(product)})">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </td>
                        `;
                        productTable.appendChild(row);
                    }
                });
            } else {
                productTable.innerHTML = `
                    <tr>
                        <td colspan="6" class="text-center text-muted py-3">No products found</td>
                    </tr>
                `;
            }
        }

        // Filter customers
        document.getElementById('customerSearch').addEventListener('input', function() {
            filterCustomers();
        });

        document.getElementById('statusFilter').addEventListener('change', function() {
            filterCustomers();
        });

        document.getElementById('clearCustomerFilters').addEventListener('click', function() {
            document.getElementById('customerSearch').value = '';
            document.getElementById('statusFilter').value = '';
            filterCustomers();
        });

        function filterCustomers() {
            const searchTerm = document.getElementById('customerSearch').value.toLowerCase();
            const statusFilter = document.getElementById('statusFilter').value;
            
            const customerTable = document.getElementById('customerTable');
            customerTable.innerHTML = '';
            
            let filteredCustomers = customers;
            
            if (searchTerm) {
                filteredCustomers = filteredCustomers.filter(customer => 
                    customer && (
                        (customer.code && customer.code.toLowerCase().includes(searchTerm)) ||
                        (customer.name && customer.name.toLowerCase().includes(searchTerm)) ||
                        (customer.shop && customer.shop.toLowerCase().includes(searchTerm))
                    )
                );
            }
            
            if (statusFilter) {
                filteredCustomers = filteredCustomers.filter(customer => 
                    customer && customer.status === statusFilter
                );
            }
            
            if (filteredCustomers.length > 0) {
                filteredCustomers.forEach((customer, index) => {
                    if (customer) {
                        const statusBadge = customer.status === 'Active' ? 
                            '<span class="badge bg-success">Active</span>' : 
                            '<span class="badge bg-secondary">Inactive</span>';
                        
                        const row = document.createElement('tr');
                        row.innerHTML = `
                            <td>${customer.code || 'N/A'}</td>
                            <td>${customer.name || 'N/A'}</td>
                            <td>${customer.shop || 'N/A'}</td>
                            <td>${customer.phone || 'N/A'}</td>
                            <td>${statusBadge}</td>
                            <td class="action-buttons">
                                <button class="btn btn-sm btn-warning" onclick="editCustomer(${customers.indexOf(customer)})">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="btn btn-sm btn-danger" onclick="deleteCustomer(${customers.indexOf(customer)})">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </td>
                        `;
                        customerTable.appendChild(row);
                    }
                });
            } else {
                customerTable.innerHTML = `
                    <tr>
                        <td colspan="6" class="text-center text-muted py-3">No customers found</td>
                    </tr>
                `;
            }
        }

        // Filter khatta entries
        document.getElementById('khattaCustomerFilter').addEventListener('change', function() {
            loadKhattaData();
        });

        // Edit product (placeholder function)
        window.editProduct = function(index) {
            alert('Edit product functionality would be implemented here');
        };

        // Delete product
        window.deleteProduct = function(index) {
            if (confirm('Are you sure you want to delete this product?')) {
                products.splice(index, 1);
                database.ref('products').set(products)
                    .then(() => {
                        alert('Product deleted successfully!');
                        loadProducts();
                        hideFirebaseError();
                    })
                    .catch(error => {
                        if (error.code === 'PERMISSION_DENIED') {
                            showFirebaseError('Cannot delete product: ' + error.message);
                            alert('Error: Cannot delete product due to Firebase permissions. Please check security rules.');
                        } else {
                            alert('Error deleting product: ' + error.message);
                        }
                    });
            }
        };

        // Edit customer (placeholder function)
        window.editCustomer = function(index) {
            alert('Edit customer functionality would be implemented here');
        };

        // Delete customer
        window.deleteCustomer = function(index) {
            if (confirm('Are you sure you want to delete this customer?')) {
                customers.splice(index, 1);
                database.ref('customers').set(customers)
                    .then(() => {
                        alert('Customer deleted successfully!');
                        loadCustomers();
                        hideFirebaseError();
                    })
                    .catch(error => {
                        if (error.code === 'PERMISSION_DENIED') {
                            showFirebaseError('Cannot delete customer: ' + error.message);
                            alert('Error: Cannot delete customer due to Firebase permissions. Please check security rules.');
                        } else {
                            alert('Error deleting customer: ' + error.message);
                        }
                    });
            }
        };

        // Delete purchase order
        window.deletePurchase = function(index) {
            if (confirm('Are you sure you want to delete this purchase order?')) {
                purchases.splice(index, 1);
                database.ref('purchases').set(purchases)
                    .then(() => {
                        alert('Purchase order deleted successfully!');
                        loadPurchases();
                        hideFirebaseError();
                    })
                    .catch(error => {
                        if (error.code === 'PERMISSION_DENIED') {
                            showFirebaseError('Cannot delete purchase: ' + error.message);
                            alert('Error: Cannot delete purchase due to Firebase permissions. Please check security rules.');
                        } else {
                            alert('Error deleting purchase order: ' + error.message);
                        }
                    });
            }
        };

        // View bill (placeholder function)
        window.viewBill = function(index) {
            alert('View bill functionality would be implemented here');
        };

        // Delete bill
        window.deleteBill = function(index) {
            if (confirm('Are you sure you want to delete this bill?')) {
                bills.splice(index, 1);
                database.ref('bills').set(bills)
                    .then(() => {
                        alert('Bill deleted successfully!');
                        loadBillingData();
                        loadDashboardData();
                        hideFirebaseError();
                    })
                    .catch(error => {
                        if (error.code === 'PERMISSION_DENIED') {
                            showFirebaseError('Cannot delete bill: ' + error.message);
                            alert('Error: Cannot delete bill due to Firebase permissions. Please check security rules.');
                        } else {
                            alert('Error deleting bill: ' + error.message);
                        }
                    });
            }
        };

        // Delete khatta entry
        window.deleteKhattaEntry = function(index) {
            if (confirm('Are you sure you want to delete this khatta entry?')) {
                khattaEntries.splice(index, 1);
                database.ref('khatta').set(khattaEntries)
                    .then(() => {
                        alert('Khatta entry deleted successfully!');
                        loadKhattaData();
                        hideFirebaseError();
                    })
                    .catch(error => {
                        if (error.code === 'PERMISSION_DENIED') {
                            showFirebaseError('Cannot delete khatta entry: ' + error.message);
                            alert('Error: Cannot delete khatta entry due to Firebase permissions. Please check security rules.');
                        } else {
                            alert('Error deleting khatta entry: ' + error.message);
                        }
                    });
            }
        };

        // Backup database (placeholder function)
        document.getElementById('backupDbBtn').addEventListener('click', function() {
            alert('Database backup functionality would be implemented here');
        });

        // Restore database (placeholder function)
        document.getElementById('restoreDbBtn').addEventListener('click', function() {
            alert('Database restore functionality would be implemented here');
        });
    </script>
</body>
</html>
