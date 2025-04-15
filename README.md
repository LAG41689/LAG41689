import React, { useState, useEffect } from 'react';
import axios from 'axios';

function App() {
  const [trades, setTrades] = useState([]);
  const [newTrade, setNewTrade] = useState({
    symbol: '',
    type: '',
    lotSize: '',
    entryPrice: ''
  });

  const token = localStorage.getItem('token');

  useEffect(() => {
    if (token) {
      axios
        .get('https://your-backend-url/api/trades', {
          headers: {
            Authorization: `Bearer ${token}`,
          },
        })
        .then((response) => setTrades(response.data))
        .catch((error) => console.error(error));
    }
  }, [token]);

  const handleSubmit = (e) => {
    e.preventDefault();
    axios
      .post(
        'https://your-backend-url/api/trades',
        newTrade,
        {
          headers: {
            Authorization: `Bearer ${token}`,
          },
        }
      )
      .then((response) => {
        setTrades([...trades, response.data]);
        setNewTrade({
          symbol: '',
          type: '',
          lotSize: '',
          entryPrice: ''
        });
      })
      .catch((error) => console.error(error));
  };

  return (
    <div>
      <h1>Copy Trader</h1>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          placeholder="Symbol"
          value={newTrade.symbol}
          onChange={(e) => setNewTrade({ ...newTrade, symbol: e.target.value })}
        />
        <input
          type="text"
          placeholder="Type"
          value={newTrade.type}
          onChange={(e) => setNewTrade({ ...newTrade, type: e.target.value })}
        />
        <input
          type="number"
          placeholder="Lot Size"
          value={newTrade.lotSize}
          onChange={(e) => setNewTrade({ ...newTrade, lotSize: e.target.value })}
        />
        <input
          type="number"
          placeholder="Entry Price"
          value={newTrade.entryPrice}
          onChange={(e) => setNewTrade({ ...newTrade, entryPrice: e.target.value })}
        />
        <button type="submit">Post Trade</button>
      </form>
      <h2>Trades:</h2>
      <ul>
        {trades.map((trade) => (
          <li key={trade._id}>
            {trade.symbol} - {trade.type} - {trade.lotSize} - {trade.entryPrice}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default App;
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);{
  "name": "copy-trader-client",
  "version": "0.1.0",
  "main": "src/index.js",
  "dependencies": {
    "axios": "^0.24.0",
    "react": "^17.0.2",
    "react-dom": "^17.0.2",
    "react-scripts": "^4.0.3"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "eslintConfig": {
    "extends": [
      "react-app",
      "react-app/jest"
    ]
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
}{
  "name": "copy-trader-server",
  "version": "1.0.0",
  "main": "index.js",
  "dependencies": {
    "cors": "^2.8.5",
    "dotenv": "^16.0.3",
    "express": "^4.18.2",
    "jsonwebtoken": "^9.0.0",
    "mongoose": "^7.3.4"
  },
  "scripts": {
    "start": "node index.js"
  }
}
