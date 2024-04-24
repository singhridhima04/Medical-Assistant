# Medical-Assistant

// server.js
const express = require('express');
const mongoose = require('mongoose');

const app = express();

// Connect to MongoDB
mongoose.connect('mongodb://localhost:27017/healthcare', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});

// Define a schema for the doctor
const doctorSchema = new mongoose.Schema({
  name: String,
  specialty: String,
  schedule: String,
  rating: Number,
});

// Create a model for the doctor
const Doctor = mongoose.model('Doctor', doctorSchema);

// Add some mock data
Doctor.insertMany([
  {
    name: 'Dr. Smith',
    specialty: 'Cardiology',
    schedule: '9:00-17:00',
    rating: 4.5,
  },
  // Add more doctors
]);

// Start the server
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Server started on port ${PORT}`));


//Backend Development:
//Create a RESTful API using Node.js and Express:

// server.js
// ...

// Add a route for doctor recommendations
app.get('/recommendations', async (req, res) => {
  // Get user symptoms from the request body
  const { symptoms } = req.query;

  // Analyze user symptoms using symptom data
  // ...

  // Find doctors with matching specialties
  const doctors = await Doctor.find({ specialty: { $in: symptoms } }).exec();

  // Filter doctors based on schedule
  // ...

  // Return the recommended doctors
  res.json(doctors);
});

// ...

// App.js
import React, { useState } from 'react';
import { useDispatch } from 'react-redux';
import axios from 'axios';

const App = () => {
  const dispatch = useDispatch();
  const [symptoms, setSymptoms] = useState('');
  const [doctors, setDoctors] = useState([]);

  const handleSubmit = async (e) => {
    e.preventDefault();

    const response = await axios.get('/recommendations', {
      params: { symptoms },
    });

    setDoctors(response.data);
  };

  return (
    <div>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={symptoms}
          onChange={(e) => setSymptoms(e.target.value)}
        />
        <button type="submit">Get Recommendations</button>
      </form>
      {doctors.map((doctor) => (
        <div key={doctor.name}>
          <h2>{doctor.name}</h2>
          <p>Specialty: {doctor.specialty}</p>
          <p>Schedule: {doctor.schedule}</p>
          <p>Rating: {doctor.rating}</p>
        </div>
      ))}
    </div>
  );
};

export default App;



//Frontend Development:
//Create a user-friendly interface using React and Redux:

// App.js
import React, { useState } from 'react';
import { useDispatch } from 'react-redux';
import axios from 'axios';

const App = () => {
  const dispatch = useDispatch();
  const [symptoms, setSymptoms] = useState('');
  const [doctors, setDoctors] = useState([]);

  const handleSubmit = async (e) => {
    e.preventDefault();

    const response = await axios.get('/recommendations', {
      params: { symptoms },
    });

    setDoctors(response.data);
  };

  return (
    <div>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={symptoms}
          onChange={(e) => setSymptoms(e.target.value)}
        />
        <button type="submit">Get Recommendations</button>
      </form>
      {doctors.map((doctor) => (
        <div key={doctor.name}>
          <h2>{doctor.name}</h2>
          <p>Specialty: {doctor.specialty}</p>
          <p>Schedule: {doctor.schedule}</p>
          <p>Rating: {doctor.rating}</p>
        </div>
      ))}
    </div>
  );
};

export default App;


