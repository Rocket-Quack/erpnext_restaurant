<div align="center">
  <p>
    <img src="docs/assets/RESTAURANT_APP_LOGO.png" alt="ERPNext Restaurant Logo" width="164"/>
  </p>
    <h1>ERPNext Restaurant<br> (WORK IN PROGRESS)</h1>
</div>

## Supported Versions

| ERPNext | Frappe | Support-Status |
|---------|--------|----------------|
| v16 Beta    | v16 Beta   | ⚙️ Coming soon     |
| v15     | v15    | ⚙️ Coming soon     |

## Installation (Frappe Cloud)

The app can be installed directly via Frappe Cloud:

1. Open the Frappe Cloud dashboard at [https://frappecloud.com/dashboard/#/sites](https://frappecloud.com/dashboard/#/sites)
2. Click **“New Site”** to create a new instance
3. In the **“Select apps to install”** step:

   * Choose the desired Frappe/ERPNext version
   * Also enable the **`ERPNEXT Restaurant`** app
4. Finish the wizard until the site has been created

## Installation (Self-Hosted)

Once ERPNext is installed, add the app to your Bench environment using the following command:

```bash
bench get-app https://github.com/Rocket-Quack/erpnext_restaurant --branch version-15
```

Install required modules and dependencies:

```bash
bench setup requirements
```

Then you can install the app on a site:

```bash
bench --site yoursite.com install-app erpnext_restaurant
```

Migrate new Installed App
```bash
bench --site yoursite.com migrate
```

## License

Copyright (C) 2026 RocketQuackIT

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

GNU GPL V3. See the LICENSE file for more information.