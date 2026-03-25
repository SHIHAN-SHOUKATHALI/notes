server.js-

const express = require('express');
const mongoose = require('mongoose');
const cors = require('cors');

const Product = require('./models/Product');

const app = express();

app.use(cors());
app.use(express.json());

mongoose.connect("mongodb://localhost:27017/productDB")
  .then(() => console.log("Mongo connected"))
  .catch(err => console.log(err));

app.post("/add-product", async (req, res)=>{
  try {
    const product = new Product(req.body);
    await product.save();
    res.json({message:"added successfully"});
  } catch (err) {
    res.status(500).json({ error: err.message});
  }
});



app.get("/Products", async (req, res)=>{
  try {
    const products= await product.find();
    res.json(products);
  }catch (err){
    res.status(500).json({ error: err.message});
  }
});




app.put("update-price/:id", async (req, res)=> {
  try {
    await Product.findByIdAndUpdate(req.params.id, {price: req.body.price});
  res.json({message:"price updated"});

  }catch (err){
    res.status(500).json({error: err.message});
  }
});


app.delete("/delete-product/:id", async (req, res)=>{
  try {
    await Product.findByIdAndDelete(req.params.id);
    res.json({message: "deleted"});
  }catch (err){
    res.status(500).json({error: err.message});

  }
});




app.listen(5001, () => console.log("Server running on port 5001"));







product.js-


const mongoose =require('mongoose');

const productSchema= new mongoose.Schema
({
    Pname:String,
    Price:Number,
    Quantity:Number,
    Category:String
});

module.exports=mongoose.model('Product',productSchema);













