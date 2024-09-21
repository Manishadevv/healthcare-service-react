# healthcare-service-react
 A simple React web application to manage and display a list of healthcare services. Users should be able to add, update, and delete services.
Here’s the `README.md` for your GitHub repository that includes the entire code for the project (`index.js`, `index.css`, `App.js`, `ServiceList.js`, and `ServiceForm.js`), along with instructions for running and deploying the project.

---

# Healthcare Services Management Application

This is a React web application to manage and display a list of healthcare services. Users can add, update, and delete services, each containing a name, description, and price.

## Table of Contents
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Technologies Used](#technologies-used)
- [Code](#code)
  - [index.js](#indexjs)
  - [index.css](#indexcss)
  - [App.js](#appjs)
  - [ServiceList.js](#servicelistjs)
  - [ServiceForm.js](#serviceformjs)
---



## Features

- **Service List:** Display a list of healthcare services.
- **Add Service:** Users can add a new service using a form.
- **Update Service:** Edit the details of an existing service.
- **Delete Service:** Remove a service from the list.

---

## Installation

To run the project locally, follow these steps:

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/healthcare-services-app.git
cd healthcare-services-app
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Run the Application

```bash
npm start
```

The application will start at `http://localhost:3000`.

---

## Usage

1. **Add a Service:**
   - Fill in the service name, description, and price.
   - Click "Add Service" to add the service to the list.

2. **Update a Service:**
   - Click the "Edit" button on a service to modify its details.
   - Update the service details and click "Update".

3. **Delete a Service:**
   - Click the "Delete" button to remove a service.

---

## Technologies Used

- **React:** Front-end library for building user interfaces.
- **React Hooks:** State management using `useState` and `useEffect`.
- **CSS:** Styling for the application.

---

## Code

### index.js

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css'; // Optional: Add styling if you have a CSS file

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```

### index.css

```css
/* General page styling */
body {
  font-family: 'Arial', sans-serif;
  background-color: #f4f4f9;
  color: #333;
  margin: 0;
  padding: 0;
  transition: background-color 0.3s ease;
}

.App {
  text-align: center;
  margin: 20px;
  padding: 20px;
  max-width: 800px;
  margin-left: auto;
  margin-right: auto;
  background-color: #ffffff;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

input {
  width: 100%;
  margin: 10px 0;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 16px;
}

button {
  padding: 12px 20px;
  margin: 10px 5px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
}

ul {
  list-style-type: none;
  padding: 0;
  margin: 20px 0;
}

li {
  background-color: #fff;
  margin: 10px 0;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 10px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  text-align: left;
}

li h3 {
  margin: 0;
  font-size: 22px;
  color: #333;
}

li p {
  margin: 5px 0;
  color: #666;
}
```

### App.js

```javascript
import React, { useState } from 'react';
import ServiceList from './components/ServiceList';
import ServiceForm from './components/ServiceForm';

function App() {
  const [services, setServices] = useState([
    { id: 1, name: 'Consultation', description: 'General health check-up', price: 50 },
    { id: 2, name: 'Dental Cleaning', description: 'Teeth cleaning', price: 80 },
    { id: 3, name: 'Physiotherapy', description: 'Physical therapy session', price: 120 },
  ]);

  const addService = (newService) => {
    setServices([...services, { ...newService, id: Date.now() }]);
  };

  const updateService = (updatedService) => {
    setServices(
      services.map((service) =>
        service.id === updatedService.id ? updatedService : service
      )
    );
  };

  const deleteService = (id) => {
    setServices(services.filter((service) => service.id !== id));
  };

  return (
    <div className="App">
      <h1>Healthcare Services</h1>
      <ServiceForm addService={addService} />
      <ServiceList
        services={services}
        updateService={updateService}
        deleteService={deleteService}
      />
    </div>
  );
}

export default App;
```

### ServiceList.js

```javascript
import React, { useState } from 'react';

function ServiceList({ services, updateService, deleteService }) {
  const [editingService, setEditingService] = useState(null);
  const [editedService, setEditedService] = useState({
    id: '',
    name: '',
    description: '',
    price: '',
  });

  const handleEditClick = (service) => {
    setEditingService(service.id);
    setEditedService({ ...service });
  };

  const handleUpdate = () => {
    updateService(editedService);
    setEditingService(null);
  };

  return (
    <div>
      <h2>Service List</h2>
      {services.length === 0 && <p>No services available</p>}
      <ul>
        {services.map((service) => (
          <li key={service.id}>
            {editingService === service.id ? (
              <div>
                <input
                  type="text"
                  value={editedService.name}
                  onChange={(e) =>
                    setEditedService({ ...editedService, name: e.target.value })
                  }
                />
                <input
                  type="text"
                  value={editedService.description}
                  onChange={(e) =>
                    setEditedService({
                      ...editedService,
                      description: e.target.value,
                    })
                  }
                />
                <input
                  type="number"
                  value={editedService.price}
                  onChange={(e) =>
                    setEditedService({ ...editedService, price: e.target.value })
                  }
                />
                <button onClick={handleUpdate}>Update</button>
              </div>
            ) : (
              <div>
                <h3>{service.name}</h3>
                <p>{service.description}</p>
                <p>Price: ${service.price}</p>
                <button onClick={() => handleEditClick(service)}>Edit</button>
                <button onClick={() => deleteService(service.id)}>Delete</button>
              </div>
            )}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default ServiceList;
```

### ServiceForm.js

```javascript
import React, { useState } from 'react';

function ServiceForm({ addService }) {
  const [service, setService] = useState({
    name: '',
    description: '',
    price: '',
  });

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!service.name || !service.description || !service.price) {
      alert('All fields are required');
      return;
    }
    addService(service);
    setService({ name: '', description: '', price: '' });
  };

  return (
    <div>
      <h2>Add New Service</h2>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          placeholder="Service Name"
          value={service.name}
          onChange={(e) => setService({ ...service, name: e.target.value })}
        />
        <input
          type="text"
          placeholder="Description"
          value={service.description}
          onChange={(e) =>
            setService({ ...service, description: e.target.value })
          }
        />
        <input
          type="number"
          placeholder="Price"
          value={service.price}
          onChange={(e) => set

Service({ ...service, price: e.target.value })}
        />
        <button type="submit">Add Service</button>
      </form>
    </div>
  );
}

export default ServiceForm;
```

---
