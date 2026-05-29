# Open Questions

## 1. What steps are missing to industrialize such solution further?

The current implementation successfully demonstrates a Bronze-Silver-Gold medallion architecture using Databricks and Spark. However, several additional steps would be required to industrialize the solution for a production environment.

### Orchestration
The notebooks should be orchestrated using a workflow orchestration tool such as:
- Databricks Workflows
- Apache Airflow
- Azure Data Factory

This would allow pipeline execution, scheduling, dependency management, retries, and monitoring.

### CI/CD
A CI/CD pipeline should be implemented using tools such as:
- GitHub Actions
- Azure DevOps
- GitLab CI/CD

The pipeline would automate:
- code validation
- unit testing
- deployment
- environment promotion

### Data Quality Checks
Data validation should be implemented to ensure:
- no invalid records
- mandatory fields are populated
- schema consistency
- duplicate detection
- NULL checks

Tools such as Great Expectations or dbt tests could be used. 
Data quality checks should be added in the silver layer because it is the standardized business-ready transformation layer. 

### Monitoring and Alerting
Monitoring should be added for:
- job failures
- SLA breaches
- performance issues
- data anomalies

Alerts can be integrated with email, Slack, or monitoring platforms.

### Incremental Processing
The current solution processes full datasets. In production, incremental processing should be implemented using:
- watermarking
- change data capture
- incremental merges

This improves performance and reduces processing costs.

### Security and Governance
Production solutions should include:
- role-based access control(RBAC)
- secret management
- Data encryption and network security
- Unity Catalog governance(metadata management, lineage tracking, access policies)
- audit logging

### Data Governance
Unity Catalog should be used for:
- centralized access management
- data lineage
- data discovery
- auditing

Delta Lake features such as:
- schema evolution
- time travel
- ACID transactions

should also be leveraged.

### Testing
Automated testing should be implemented, including:
- unit testing
- integration testing
- data reconciliation testing

### Infrastructure Separation
Separate environments should be maintained:
- Development
- Testing
- Production

This improves deployment safety and maintainability.

---

## 2. If the solution was implemented in dbt-core, how would the overall architecture change? Would there be another cloud resources needed?

If the transformation layer was implemented using dbt-core, the architecture would shift toward a SQL-driven transformation framework.
Dbt-core would orchestrate and manage SQL transformations while Databricks would remain the underlying execution engine.

### Architecture Changes

The Bronze ingestion layer would still remain in Spark/Databricks because dbt-core is primarily designed for transformations rather than ingestion.

The architecture would become:

- Bronze Layer → Spark ingestion
- Silver Layer → dbt models
- Gold Layer → dbt marts

dbt-core would manage:
- SQL transformations
- model dependencies
- testing
- documentation
- lineage

### Additional Cloud Resources

Some additional resources may be required depending on deployment strategy:
- Compute resource to run dbt-core
- CI/CD runner
- Orchestration platform
- Storage for dbt artifacts and logs
- Monitoring and logging tools

Examples:
- Virtual Machine
- Kubernetes
- Azure Container Apps
- GitHub Actions runners

Databricks would still be required as the processing engine.

---

## 3. What would implementation in dbt-core bring to the project? What would be the upsides and downsides?

Implementing the solution in dbt-core would bring several improvements related to maintainability, testing, documentation, and transformation management.

### Upsides

#### Better Maintainability
dbt encourages modular SQL transformations using reusable models.

#### Built-in Documentation
dbt automatically generates:
- lineage graphs
- model documentation
- dependency visualization

#### Testing Framework
dbt provides built-in testing capabilities for:
- uniqueness
- null validation
- referential integrity

#### Improved Collaboration
Analytics engineers and data engineers can collaborate more efficiently using SQL-based transformations.

#### CI/CD Friendly
dbt integrates very well with Git-based workflows and automated deployments.

### Downsides

#### Additional Complexity
Introducing dbt adds another framework that must be maintained and learned.

#### SQL-Centric Approach
Complex Python-based transformations may be harder to implement compared to Spark DataFrame APIs.

#### Additional Infrastructure
dbt-core requires separate execution infrastructure and orchestration.

#### Learning Curve
Team members unfamiliar with dbt may require onboarding and training.

---

## 4. Please estimate the effort you’d request to implement the solution in dbt-core.

The estimated effort for implementing the current solution in dbt-core would be approximately:

| Task | Estimated Effort |
|---|---|
| Environment Setup | 0.5 day |
| dbt Project Structure | 0.5 day |
| Silver Models Migration | 1 day |
| Gold Models Migration | 0.5 day |
| Testing and Documentation | 0.5 day |
| CI/CD Integration | 0.5 - 1 day |

### Total Estimated Effort
Approximately 3–4 working days.

The estimate assumes:
- existing Databricks environment
- no major infrastructure changes
- small-to-medium dataset size
- standard CI/CD requirements
