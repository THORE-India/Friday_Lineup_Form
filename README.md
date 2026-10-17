# THORE PULSE — Friday Lineup

**A Sales Operations & Performance Management Application**

THORE PULSE — Friday Lineup is a lightweight web application developed for **The House of Real Estate (THORE)** to streamline weekly sales visit planning, capture customer engagement information, and support operational reporting through a centralized web interface.

The application combines a browser-based frontend with cloud-based services to support sales operations and related marketing activities.

## Overview

The application is designed to simplify the collection and management of sales visit information while providing an administrative interface for operational monitoring and reporting.

Built using standard web technologies, the frontend can be hosted on a static web hosting platform and integrated with the required backend services.

## Key Features

### Sales Visit Management
- Digital form for recording customer visit information.
- Structured collection of relevant sales and project details.
- Client-side input validation and user-friendly form controls.
- Responsive interface for desktop and mobile browsers.

### Operational Dashboard
- Dashboard interface for operational reporting and monitoring.
- Data visualization through charts and tabular views.
- Access to relevant information based on the application's configured authorization mechanisms.

### Marketing Workflow Support
- Supporting interfaces for marketing-related operational activities.
- Integration with external services for selected data operations.
- Centralized browser-based access to relevant application functions.

### User Experience
- Lightweight frontend with no mandatory application build process.
- Browser-based interface using standard HTML, CSS, and JavaScript.
- Integration with cloud services for data storage and application functionality.

## Technology Stack

| Technology | Purpose |
|---|---|
| HTML5 | Application structure |
| CSS3 | Styling and responsive layouts |
| JavaScript (ES6+) | Client-side application logic |
| Supabase | Backend services, authentication, and data operations |
| Chart.js | Data visualization |
| Flatpickr | Date and time selection |
| Google Apps Script | Integration with selected external workflows |

The actual functionality available depends on the configured backend services, permissions, and deployment environment.

## High-Level Architecture

The application follows a lightweight frontend architecture.

```text
                 THORE PULSE
                      |
          Browser-Based Frontend
                      |
          +-----------+-----------+
          |           |           |
       Forms      Dashboard   Supporting
                              Interfaces
          |
     Cloud Services
          |
     +----+----------------+
     |                     |
  Supabase          External Services
```

The frontend communicates with configured backend services to support data operations and application functionality. Authentication, data access permissions, and backend integrations must be configured appropriately for the deployment environment.

This diagram provides a conceptual overview and does not represent every internal component or integration.

## Repository Structure

The repository contains the frontend pages and supporting resources required by the application.

| File | Description |
|---|---|
| `index.html` | Main sales visit entry interface |
| `dashboard.html` | Administrative and operational dashboard |
| `marketing.html` | Supporting marketing interface |
| `closed.html` | Access-window status page |
| `no_authorization.html` | Authorization-related page |
| `LICENSE` | Repository license information |

The exact behaviour of each component depends on the current application implementation and its configured services.

## Prerequisites

To preview or work with the frontend, you will generally need:

- A modern web browser with JavaScript enabled.
- Python 3 or another lightweight local HTTP server.
- Internet connectivity for externally hosted libraries and configured backend services.
- Appropriate backend configuration and permissions for features requiring live data.

Some application features will not operate fully without the required backend services.

## Local Development and Preview

The application uses standard web technologies and does not require a frontend compilation step for basic local preview.

### 1. Obtain the repository

Clone the repository or download its source files from GitHub.

```bash
git clone <YOUR_REPOSITORY_URL>
cd <YOUR_REPOSITORY_DIRECTORY>
```

Replace the placeholders with your repository URL and local directory name.

### 2. Start a local HTTP server

If Python 3 is installed, run the following command from the repository root:

```bash
python -m http.server 8000
```

### 3. Open the application

Visit the following address in your browser:

```text
http://localhost:8000/
```

To open a particular page directly, append its filename to the address.

**Note:** Local preview does not automatically configure authentication, backend connectivity, permissions, or external integrations. Features that depend on these services require a valid development environment.

## Configuration

The application depends on its configured cloud services and external integrations.

Before running the application in a development or production environment:

1. Verify that the required backend services are available.
2. Confirm that authentication and authorization are configured appropriately.
3. Ensure that data access permissions follow the principle of least privilege.
4. Verify that external integrations accept only authorized requests.
5. Keep privileged credentials and application secrets outside browser-delivered code.
6. Test all required functionality in a non-production environment before deployment.

Configuration procedures may differ between environments. Refer to the relevant service documentation and approved internal deployment instructions when setting up a new instance.

## Deployment

The frontend can be deployed to a static hosting provider compatible with HTML, CSS, and JavaScript.

Potential hosting options include:

- GitHub Pages
- Netlify
- Vercel
- Other compatible static web hosting platforms

Deployment requires the appropriate backend configuration and access policies.

Before publishing or deploying:

- Review the repository for accidentally committed credentials.
- Confirm that database access policies are correctly configured.
- Ensure that privileged operations are handled by appropriately secured backend services.
- Validate authentication and authorization behaviour.
- Test the application using the intended deployment URL.
- Confirm that external integrations function correctly in the target environment.

Static hosting alone does not provide server-side protection for secrets or privileged operations.

## Security and Privacy

Security depends on the application code, backend configuration, access policies, and external services.

The following practices are recommended:

- **Credential protection:** Never expose passwords, service-role keys, private tokens, or application secrets in client-side files.
- **Data access:** Apply appropriate backend access controls and validate permissions for all sensitive operations.
- **Authentication:** Ensure that protected functionality requires the appropriate authentication.
- **External integrations:** Validate incoming requests and protect privileged operations using server-side authorization.
- **Data privacy:** Avoid exposing customer information, employee information, or confidential business data through public interfaces or repositories.
- **Repository hygiene:** Review code, configuration files, commit history, and deployment settings before making the repository public.

Supabase publishable or legacy anonymous keys may be used in browser applications when designed for that purpose, but they must be supported by correctly configured authorization policies. Such keys should not be treated as a substitute for access control.

If a credential or application secret has already been exposed, assess its validity and impact, revoke or rotate it where necessary, and investigate possible unauthorized use.

## Limitations

The application's available capabilities depend on its current implementation and the services configured for the deployment.

General considerations include:

- Backend-dependent features require network connectivity and available services.
- Authentication and data access depend on the configured authorization mechanisms.
- External integrations may have independent availability and configuration requirements.
- Local preview does not reproduce every aspect of the production environment.
- Database provisioning, access policies, and backend service configuration may require separate setup.

These considerations should be evaluated when creating a new development or deployment environment.

## Contributing

Changes should preserve the application's existing behaviour unless a change is explicitly intended.

For proposed improvements:

1. Review the existing implementation before making modifications.
2. Test affected functionality in a development environment.
3. Avoid committing credentials, customer information, or confidential configuration.
4. Document relevant changes clearly.
5. Review the final changes before merging them into the main branch.

Repository contribution permissions and review requirements should follow the project's applicable access policies.

## License

Refer to the [`LICENSE`](LICENSE) file included in this repository for the applicable license terms.

## Project Information

**Organization:** The House of Real Estate (THORE)

**Project:** THORE PULSE — Friday Lineup

This repository contains frontend components for an internal sales operations application. Access to associated data and services is subject to the relevant authorization and deployment configuration.
