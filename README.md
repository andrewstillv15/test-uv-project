# Store Inventory Management Service

A Python-based REST API service that helps store owners and managers efficiently track, manage, and optimize their inventory operations.

## Features

- **Product Management**: Add, update, and remove products from inventory
- **Stock Tracking**: Real-time inventory levels with automatic low-stock alerts
- **Category Organization**: Organize products by categories for better management
- **Supplier Management**: Track supplier information and purchase orders
- **Reporting**: Generate inventory reports and analytics
- **Barcode Integration**: Support for barcode scanning and generation
- **Multi-location Support**: Manage inventory across multiple store locations
- **API-First Design**: RESTful API for easy integration with existing systems

## Technology Stack

- **Framework**: FastAPI
- **Database**: PostgreSQL with SQLAlchemy ORM
- **Authentication**: JWT-based authentication
- **Documentation**: Auto-generated OpenAPI/Swagger documentation
- **Testing**: pytest with comprehensive test coverage
- **Deployment**: Docker containerization support

## Quick Start

### Prerequisites

- Python 3.8 or higher
- PostgreSQL 12+
- pip or poetry for dependency management

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-org/inventory-management-service.git
   cd inventory-management-service
   ```

2. **Create a virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Set up environment variables**
   ```bash
   cp .env.example .env
   # Edit .env with your database credentials and configuration
   ```

5. **Initialize the database**
   ```bash
   python -m alembic upgrade head
   ```

6. **Run the service**
   ```bash
   python -m uvicorn app.main:app --reload
   ```

The service will be available at `http://localhost:8000`

### Docker Setup

1. **Build and run with Docker Compose**
   ```bash
   docker-compose up -d
   ```

This will start the application and PostgreSQL database in containers.

## Configuration

The service uses environment variables for configuration. Key variables include:

```env
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/inventory_db

# Security
SECRET_KEY=your-secret-key-here
ACCESS_TOKEN_EXPIRE_MINUTES=30

# Application
DEBUG=False
API_V1_STR=/api/v1
PROJECT_NAME=Inventory Management Service

# Email notifications (optional)
SMTP_HOST=smtp.gmail.com
SMTP_USER=your-email@gmail.com
SMTP_PASSWORD=your-app-password
```

## API Documentation

Once the service is running, you can access:

- **Interactive API Docs**: `http://localhost:8000/docs`
- **OpenAPI Schema**: `http://localhost:8000/openapi.json`

### Key Endpoints

#### Authentication
- `POST /api/v1/auth/login` - User login
- `POST /api/v1/auth/register` - User registration

#### Products
- `GET /api/v1/products` - List all products
- `POST /api/v1/products` - Create new product
- `GET /api/v1/products/{product_id}` - Get product details
- `PUT /api/v1/products/{product_id}` - Update product
- `DELETE /api/v1/products/{product_id}` - Delete product

#### Inventory
- `GET /api/v1/inventory` - Get inventory levels
- `POST /api/v1/inventory/adjust` - Adjust stock levels
- `GET /api/v1/inventory/low-stock` - Get low stock items
- `POST /api/v1/inventory/restock` - Record restocking

#### Reports
- `GET /api/v1/reports/inventory-summary` - Inventory summary report
- `GET /api/v1/reports/stock-movement` - Stock movement history
- `GET /api/v1/reports/valuation` - Inventory valuation report

## Usage Examples

### Creating a Product

```python
import requests

# Create a new product
product_data = {
    "name": "Wireless Headphones",
    "sku": "WH001",
    "description": "High-quality wireless headphones",
    "category_id": 1,
    "unit_price": 99.99,
    "cost_price": 65.00,
    "min_stock_level": 10,
    "max_stock_level": 100
}

response = requests.post(
    "http://localhost:8000/api/v1/products",
    json=product_data,
    headers={"Authorization": "Bearer your-jwt-token"}
)
```

### Adjusting Inventory

```python
# Adjust stock levels
adjustment_data = {
    "product_id": 1,
    "quantity_change": 50,
    "adjustment_type": "restock",
    "reason": "Weekly restock delivery",
    "location_id": 1
}

response = requests.post(
    "http://localhost:8000/api/v1/inventory/adjust",
    json=adjustment_data,
    headers={"Authorization": "Bearer your-jwt-token"}
)
```

## Development

### Project Structure

```
inventory-management-service/
├── app/
│   ├── __init__.py
│   ├── main.py              # FastAPI application
│   ├── core/
│   │   ├── config.py        # Configuration settings
│   │   ├── security.py      # Authentication & security
│   │   └── database.py      # Database connection
│   ├── api/
│   │   ├── v1/
│   │   │   ├── endpoints/   # API route handlers
│   │   │   └── __init__.py
│   │   └── dependencies.py  # FastAPI dependencies
│   ├── models/
│   │   ├── product.py       # Database models
│   │   ├── inventory.py
│   │   └── user.py
│   ├── schemas/
│   │   ├── product.py       # Pydantic schemas
│   │   ├── inventory.py
│   │   └── user.py
│   └── services/
│       ├── product_service.py    # Business logic
│       ├── inventory_service.py
│       └── notification_service.py
├── tests/
├── alembic/                 # Database migrations
├── docker-compose.yml
├── Dockerfile
├── requirements.txt
└── README.md
```

### Running Tests

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=app --cov-report=html

# Run specific test file
pytest tests/test_products.py
```

### Database Migrations

```bash
# Create a new migration
alembic revision --autogenerate -m "Add new column to products"

# Apply migrations
alembic upgrade head

# Rollback migration
alembic downgrade -1
```

## Deployment

### Production Deployment

1. **Set production environment variables**
2. **Use a production WSGI server like Gunicorn**
   ```bash
   gunicorn app.main:app -w 4 -k uvicorn.workers.UvicornWorker
   ```
3. **Set up reverse proxy (nginx)**
4. **Configure SSL certificates**
5. **Set up monitoring and logging**

### Kubernetes Deployment

Example Kubernetes manifests are provided in the `k8s/` directory.

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines

- Follow PEP 8 style guidelines
- Write comprehensive tests for new features
- Update documentation for API changes
- Use type hints throughout the codebase
- Ensure all tests pass before submitting PRs

## Troubleshooting

### Common Issues

**Database Connection Error**
- Verify PostgreSQL is running
- Check database credentials in `.env` file
- Ensure database exists and user has proper permissions

**Authentication Errors**
- Verify JWT token is valid and not expired
- Check SECRET_KEY configuration
- Ensure user has proper permissions for the operation

**Low Performance**
- Check database indexes are properly created
- Monitor database query performance
- Consider implementing caching for frequently accessed data

## Support

- **Documentation**: [Full documentation](https://docs.your-service.com)
- **Issues**: [GitHub Issues](https://github.com/your-org/inventory-management-service/issues)
- **Discussions**: [GitHub Discussions](https://github.com/your-org/inventory-management-service/discussions)
- **Email**: support@your-service.com

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- FastAPI for the excellent web framework
- SQLAlchemy for robust ORM capabilities
- PostgreSQL for reliable data storage
- The open-source community for inspiration and tools

---

**Version**: 1.0.0  
**Last Updated**: August 2025
