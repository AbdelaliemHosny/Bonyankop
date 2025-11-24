# Bonyanko - Smart Home Maintenance Platform

![Project Status](https://img.shields.io/badge/status-active-success.svg)
![Database](https://img.shields.io/badge/database-PostgreSQL-336791.svg)
![Version](https://img.shields.io/badge/version-3.0-blue.svg)

## 📋 Project Overview

**Bonyanko** is a comprehensive Smart Home Maintenance Platform that connects homeowners (citizens) with verified service providers (engineers and companies) through an AI-powered diagnostic and marketplace system. The platform streamlines the process of identifying, quoting, and completing home maintenance and repair projects.

### Key Features

- 🤖 **AI-Powered Diagnostics**: Upload images of home issues for instant AI analysis and risk assessment
- 👥 **Verified Service Providers**: Connect with certified engineers and companies
- 💰 **Competitive Quotes**: Receive multiple quotes from qualified providers
- 📊 **Project Management**: Track projects from request to completion
- ⭐ **Rating & Review System**: Multi-dimensional rating system for quality assurance
- 🔒 **Secure & Auditable**: Comprehensive audit logging for transparency
- 🏆 **Provider Leaderboard**: Merit-based ranking system for service providers

## 🏗️ Architecture

### Database Design

The platform is built on a robust PostgreSQL database with **10 core tables**:

1. **users** - User authentication and profiles
2. **provider_profiles** - Extended profiles for service providers
3. **portfolio_items** - Provider work showcases
4. **diagnostics** - AI-powered problem analysis
5. **service_requests** - Citizen maintenance requests
6. **quotes** - Provider price quotes
7. **projects** - Active and completed work
8. **ratings** - Multi-dimensional reviews
9. **audit_logs** - System audit trail
10. **system_settings** - Platform configuration

### Technology Stack

- **Database**: PostgreSQL 14+
- **Extensions**: uuid-ossp, pg_trgm
- **Data Types**: JSONB for flexible schema, Custom ENUMs for type safety
- **Search**: Full-text search with GIN indexes
- **Automation**: Triggers for auto-updates and statistics

## 📊 Database Schema

### Entity Relationship Diagram

```
┌─────────────┐         ┌──────────────────┐         ┌─────────────────┐
│   USERS     │────1:1──│ PROVIDER_PROFILES│────1:M──│ PORTFOLIO_ITEMS │
└─────────────┘         └──────────────────┘         └─────────────────┘
       │                        │
       │ 1:M                    │ 1:M
       ▼                        ▼
┌─────────────┐         ┌─────────────┐
│ DIAGNOSTICS │         │   QUOTES    │
└─────────────┘         └─────────────┘
       │                        │
       │ 1:1                    │ M:1
       ▼                        ▼
┌─────────────────┐    ┌─────────────┐
│ SERVICE_REQUESTS│───▶│  PROJECTS   │
└─────────────────┘    └─────────────┘
                              │ 1:1
                              ▼
                       ┌─────────────┐
                       │   RATINGS   │
                       └─────────────┘
```

### Key Relationships

- **Users → Diagnostics**: Citizens upload images for AI analysis (1:M)
- **Diagnostics → Service Requests**: AI results inform service requests (1:1)
- **Service Requests → Quotes**: Providers submit competitive quotes (1:M)
- **Quotes → Projects**: Accepted quotes become active projects (1:1)
- **Projects → Ratings**: Completed projects receive detailed ratings (1:1)

## 🚀 Getting Started

### Prerequisites

- PostgreSQL 14 or higher
- Git
- Admin access to PostgreSQL server

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/mustafah0sny/Bonyankop.git
   cd Bonyankop
   ```

2. **Run the database setup script**
   ```bash
   psql -U postgres -f script_to_generate_database.sql
   ```

3. **Verify installation**
   ```sql
   \c smart_home_maintenance
   \dt
   ```

### Database Setup Details

The setup script automatically creates:

- ✅ Database: `smart_home_maintenance`
- ✅ 8 Custom ENUM types
- ✅ 10 Tables with constraints
- ✅ 60+ Indexes (single, composite, GIN)
- ✅ 11 Triggers for automation
- ✅ 3 Stored functions
- ✅ 3 Useful views
- ✅ 8 Default system settings

## 📖 Documentation

### Available Documentation Files

- **[DATABASE_DESIGN_WIKI.md](DATABASE_DESIGN_WIKI.md)** - Comprehensive database design documentation
- **[DATABASE_ERD.md](DATABASE_ERD.md)** - Entity Relationship Diagram with ASCII art
- **[database_schema.dbml](database_schema.dbml)** - DBML schema for dbdiagram.io
- **[class_diagram.puml](class_diagram.puml)** - PlantUML class diagram for OOP design
- **[script_to_generate_database.sql](script_to_generate_database.sql)** - Complete database setup script

### Visual Diagrams

The repository includes:
- ERD diagrams (PNG format)
- Class diagrams (PNG format)
- Original design reference images

## 🔑 Key Features Explained

### 1. AI-Powered Diagnostics

Citizens upload images of maintenance issues. The AI system:
- Analyzes the problem
- Assigns risk levels (low/medium/high)
- Categorizes the issue (plumbing, electrical, structural, HVAC, roofing)
- Provides recommended actions
- Estimates cost ranges

### 2. Service Marketplace

- Citizens post service requests based on diagnostics
- Multiple providers can submit competitive quotes
- Citizens review quotes and select providers
- Projects are created from accepted quotes

### 3. Provider Statistics (Auto-Updated)

Providers receive automatic statistics updates:
- Average rating (calculated from all reviews)
- Total projects completed
- Completion rate percentage
- Response time metrics

### 4. Multi-Dimensional Rating System

Projects are rated across 6 dimensions:
- Overall rating
- Quality of work
- Timeliness
- Professionalism
- Value for money
- Communication

### 5. Audit Trail

Complete audit logging tracks:
- User actions
- Data modifications
- System events
- Security events

## 🗄️ Database Views

### Pre-built Views for Common Queries

1. **active_service_requests** - All open requests with citizen details
2. **provider_leaderboard** - Top-rated verified providers
3. **project_summary** - Complete project overview with ratings

## 🔐 Security Features

- Password hashing (stored as hash only)
- Email validation constraints
- Phone number format validation
- Role-based access control (RBAC)
- Comprehensive audit logging
- IP address tracking
- Session management

## 📈 Performance Optimization

### Indexing Strategy

- **Single-column indexes** for frequently queried fields
- **Composite indexes** for multi-column queries
- **GIN indexes** for JSONB and full-text search
- **Partial indexes** for conditional queries
- **Unique indexes** for data integrity

### Automatic Updates via Triggers

- `updated_at` timestamps auto-update on record changes
- Provider statistics recalculate on new ratings/projects
- Quote counts update automatically on service requests

## 🛠️ Database Functions

### Available Stored Functions

```sql
-- Calculate provider average rating
SELECT calculate_provider_rating('<provider_id>');

-- Trigger function: Update provider statistics
CREATE TRIGGER update_provider_stats_on_rating ...

-- Trigger function: Update quotes count
CREATE TRIGGER update_quotes_count_trigger ...
```

## 📊 Sample Queries

### Get Top-Rated Providers
```sql
SELECT * FROM provider_leaderboard LIMIT 10;
```

### Find Active Service Requests
```sql
SELECT * FROM active_service_requests 
WHERE problem_category = 'plumbing';
```

### Search Reviews by Keyword
```sql
SELECT * FROM ratings 
WHERE to_tsvector('english', review_text) @@ to_tsquery('excellent & professional');
```

### Get Provider Performance Metrics
```sql
SELECT 
    business_name,
    average_rating,
    total_projects,
    completion_rate
FROM provider_profiles
WHERE is_verified = TRUE
ORDER BY average_rating DESC, total_projects DESC;
```

## 🔄 Workflow Example

1. **Citizen uploads image** → `diagnostics` table
2. **AI analyzes issue** → Risk level, category, recommendations
3. **Citizen creates request** → `service_requests` table
4. **Providers submit quotes** → `quotes` table
5. **Citizen accepts quote** → `projects` table created
6. **Provider completes work** → Status updated
7. **Citizen rates provider** → `ratings` table
8. **Statistics auto-update** → Provider metrics recalculated

## 📝 Data Types & Enums

### Custom ENUM Types

```sql
user_role: citizen, engineer, company, government, admin
provider_type: company, engineer
risk_level: low, medium, high
problem_category: plumbing, electrical, structural, hvac, roofing
request_status: open, quotes_received, provider_selected, cancelled
quote_status: pending, accepted, rejected, expired, withdrawn
project_status: scheduled, in_progress, completed, cancelled, disputed
payment_status: pending, partial, completed, refunded
```

## 🎯 Use Cases

### For Citizens
- Quick problem diagnosis via AI
- Compare multiple provider quotes
- Track project progress
- Rate and review completed work
- View provider portfolios

### For Service Providers
- Respond to service requests
- Submit competitive quotes
- Manage active projects
- Build portfolio with completed work
- Improve reputation through ratings

### For Platform Admins
- Monitor all platform activity
- Verify provider credentials
- Manage system settings
- Review audit logs
- Generate reports from data

## 🌟 Future Enhancements

- [ ] Real-time notifications
- [ ] Payment processing integration
- [ ] Mobile application support
- [ ] Advanced analytics dashboard
- [ ] Machine learning for quote recommendations
- [ ] Geographic service area mapping
- [ ] Multi-language support
- [ ] API documentation

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 👥 Contributors

- **Mustafa Hosny** - Initial work - [@mustafah0sny](https://github.com/mustafah0sny)

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

1. Fork the project
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📞 Support

For support, please open an issue in the GitHub repository.

## 🙏 Acknowledgments

- PostgreSQL community for excellent documentation
- dbdiagram.io for DBML support
- PlantUML for diagram generation
- AI/ML community for diagnostic inspiration

---

**Built with ❤️ for better home maintenance management**
