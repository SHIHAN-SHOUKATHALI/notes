Frontend Setup
npx create-react-app frontend
cd frontend
npm install axios react-router-dom
App.js
import { BrowserRouter as Router, Routes, Route, Link } from "react-router-dom";
import AddBook from "./AddBook";
import ViewBooks from "./ViewBooks";

function App() {
  return (
    <Router>
      <div style={{ textAlign: "center" }}>
        <h2>Book Management System</h2>
        <Link to="/">Add Book</Link> | <Link to="/view">View Books</Link>

        <Routes>
          <Route path="/" element={<AddBook />} />
          <Route path="/view" element={<ViewBooks />} />
        </Routes>
      </div>
    </Router>
  );
}

export default App;
AddBook.js
import { useState } from "react";
import axios from "axios";

function AddBook() {
  const [book, setBook] = useState({
    title: "",
    author: "",
    price: "",
    category: ""
  });

  const handleChange = (e) => {
    setBook({ ...book, [e.target.name]: e.target.value });
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    await axios.post("http://localhost:5000/add-book", book);
    alert("Book Added");
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="title" placeholder="Title" onChange={handleChange} required /><br /><br />
      <input name="author" placeholder="Author" onChange={handleChange} required /><br /><br />
      <input name="price" type="number" placeholder="Price" onChange={handleChange} required /><br /><br />
      <input name="category" placeholder="Category" onChange={handleChange} required /><br /><br />
      <button type="submit">Add Book</button>
    </form>
  );
}

export default AddBook;
ViewBooks.js
import { useEffect, useState } from "react";
import axios from "axios";

function ViewBooks() {
  const [books, setBooks] = useState([]);
  const [newPrice, setNewPrice] = useState("");

  useEffect(() => {
    fetchBooks();
  }, []);

  const fetchBooks = async () => {
    const res = await axios.get("http://localhost:5000/books");
    setBooks(res.data);
  };

  const updatePrice = async (id) => {
    await axios.put(`http://localhost:5000/update-price/${id}`, {
      price: newPrice
    });
    alert("Price Updated");
    fetchBooks();
  };

  const deleteBook = async (id) => {
    await axios.delete(`http://localhost:5000/delete-book/${id}`);
    alert("Book Deleted");
    fetchBooks();
  };

  return (
    <div>
      <h3>Book List</h3>
      {books.map(book => (
        <div key={book._id} style={{ border: "1px solid black", margin: "10px", padding: "10px" }}>
          <p>Title: {book.title}</p>
          <p>Author: {book.author}</p>
          <p>Price: {book.price}</p>
          <p>Category: {book.category}</p>

          <input
            type="number"
            placeholder="New Price"
            onChange={(e) => setNewPrice(e.target.value)}
          />
          <button onClick={() => updatePrice(book._id)}>Update Price</button>
          <button onClick={() => deleteBook(book._id)}>Delete</button>
        </div>
      ))}
    </div>
  );
}

export default ViewBooks;
