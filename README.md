<!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>USAOnly</title>
     <script src="https://cdn.jsdelivr.net/npm/react@18/umd/react.development.js"></script>
     <script src="https://cdn.jsdelivr.net/npm/react-dom@18/umd/react-dom.development.js"></script>
     <script src="https://cdn.tailwindcss.com"></script>
     <script src="https://cdn.jsdelivr.net/npm/@babel/standalone/babel.min.js"></script>
   </head>
   <body class="bg-white font-sans">
     <div id="root"></div>

     <script type="text/babel">
       const { useState, useEffect } = React;

       const initialProducts = [
         { 
           id: 1, 
           name: "American Leather Wallet", 
           price: 49.99, 
           category: "Accessories", 
           image: "https://images.unsplash.com/photo-1524578271613-d550e475a43c?ixlib=rb-4.0.3&auto=format&fit=crop&w=150&h=150&q=80&sat=-100", 
           origin: "Texas", 
           stock: 10 
         },
         { 
           id: 2, 
           name: "USA-Made Steel Hammer", 
           price: 29.99, 
           category: "Tools", 
           image: "https://images.unsplash.com/photo-1586861255460-15f846141f9e?ixlib=rb-4.0.3&auto=format&fit=crop&w=150&h=150&q=80&sat=-100", 
           origin: "Ohio", 
           stock: 15 
         },
         { 
           id: 3, 
           name: "Organic Cotton Tee", 
           price: 19.99, 
           category: "Clothing", 
           image: "https://images.unsplash.com/photo-1521572163474-6864f9cf17ab?ixlib=rb-4.0.3&auto=format&fit=crop&w=150&h=150&q=80&sat=-100", 
           origin: "California", 
           stock: 20 
         },
         { 
           id: 4, 
           name: "Handcrafted Wooden Bowl", 
           price: 34.99, 
           category: "Home", 
           image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?ixlib=rb-4.0.3&auto=format&fit=crop&w=150&h=150&q=80&sat=-100", 
           origin: "Vermont", 
           stock: 8 
         },
         { 
           id: 5, 
           name: "Patriotic Flag Mug", 
           price: 15.99, 
           category: "Home", 
           image: "https://images.unsplash.com/photo-1600585154340-be6161a56a0c?ixlib=rb-4.0.3&auto=format&fit=crop&w=150&h=150&q=80&sat=-100", 
           origin: "Pennsylvania", 
           stock: 25 
         },
       ];

       function Header({ cartCount, setSearchTerm, setShowCart }) {
         return (
           <header className="bg-black text-white p-6">
             <div className="container mx-auto flex flex-col sm:flex-row justify-between items-center">
               <h1 className="text-2xl font-light tracking-wider mb-4 sm:mb-0">USAOnly</h1>
               <div className="flex items-center space-x-6">
                 <input 
                   type="text" 
                   placeholder="Search products" 
                   className="p-2 bg-gray-900 text-white border-b border-gray-700 focus:outline-none w-64" 
                   onChange={(e) => setSearchTerm(e.target.value)}
                 />
                 <button 
                   onClick={() => setShowCart(true)} 
                   className="text-gray-300 hover:text-white"
                 >
                   Cart ({cartCount})
                 </button>
               </div>
             </div>
           </header>
         );
       }

       function ProductCard({ product, addToCart, setSelectedProduct }) {
         return (
           <div className="p-4">
             <img src={product.image} alt={product.name} className="w-full h-48 object-cover mb-4" />
             <h3 className="text-lg font-medium text-gray-900">{product.name}</h3>
             <p className="text-gray-600 text-sm">Made in {product.origin}</p>
             <p className="text-gray-900 font-semibold">${product.price.toFixed(2)}</p>
             <p className="text-gray-600 text-sm">In Stock: {product.stock}</p>
             <div className="flex space-x-4 mt-4">
               <button 
                 onClick={() => addToCart(product)} 
                 className={`text-black border border-black px-4 py-2 hover:bg-gray-200 transition ${product.stock === 0 ? 'opacity-50 cursor-not-allowed' : ''}`}
                 disabled={product.stock === 0}
               >
                 {product.stock === 0 ? 'Out of Stock' : 'Add to Cart'}
               </button>
               <button 
                 onClick={() => setSelectedProduct(product)} 
                 className="text-black border border-black px-4 py-2 hover:bg-gray-200 transition"
               >
                 Details
               </button>
             </div>
           </div>
         );
       }

       function ProductDetails({ product, setSelectedProduct }) {
         return (
           <div className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
             <div className="bg-white p-6 max-w-md w-full">
               <h2 className="text-2xl font-medium mb-4">{product.name}</h2>
               <img src={product.image} alt={product.name} className="w-full h-64 object-cover mb-4" />
               <p className="text-gray-600">Made in {product.origin}</p>
               <p className="text-gray-900 font-semibold mb-4">${product.price.toFixed(2)}</p>
               <p className="text-gray-600 mb-4">In Stock: {product.stock}</p>
               <button 
                 onClick={() => setSelectedProduct(null)} 
                 className="text-black border border-black px-4 py-2 hover:bg-gray-200 transition"
               >
                 Close
               </button>
             </div>
           </div>
         );
       }

       function Cart({ cart, setCart, setShowCart, setShowCheckout }) {
         const removeFromCart = (id) => {
           setCart(cart.filter(item => item.id !== id));
         };

         const total = cart.reduce((sum, item) => sum + item.price, 0).toFixed(2);

         return (
           <div className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
             <div className="bg-white p-6 max-w-md w-full">
               <h2 className="text-2xl font-medium mb-4">Your Cart</h2>
               {cart.length === 0 ? (
                 <p className="text-gray-600">Cart is empty</p>
               ) : (
                 <div>
                   {cart.map(item => (
                     <div key={item.id} className="flex justify-between items-center mb-2">
                       <span>{item.name} - ${item.price.toFixed(2)}</span>
                       <button 
                         onClick={() => removeFromCart(item.id)} 
                         className="text-black border border-black px-2 py-1 hover:bg-gray-200"
                       >
                         Remove
                       </button>
                     </div>
                   ))}
                   <p className="text-gray-900 font-semibold mt-4">Total: ${total}</p>
                   <button 
                     onClick={() => { setShowCart(false); setShowCheckout(true); }} 
                     className="mt-4 text-black border border-black px-4 py-2 hover:bg-gray-200 transition"
                   >
                     Proceed to Checkout
                   </button>
                 </div>
               )}
               <button 
                 onClick={() => setShowCart(false)} 
                 className="mt-4 text-black border border-black px-4 py-2 hover:bg-gray-200 transition"
               >
                 Close
               </button>
             </div>
           </div>
         );
       }

       function Checkout({ cart, setCart, setShowCheckout }) {
         const total = cart.reduce((sum, item) => sum + item.price, 0).toFixed(2);

         const handleCheckout = () => {
           alert("Thank you for your purchase! (This is a demo - no real payment processed)");
           setCart([]);
           setShowCheckout(false);
         };

         return (
           <div className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
             <div className="bg-white p-6 max-w-md w-full">
               <h2 className="text-2xl font-medium mb-4">Checkout</h2>
               <p className="text-gray-600 mb-4">Total: ${total}</p>
               <div className="space-y-4">
                 <input type="text" placeholder="Full Name" className="w-full p-2 border border-gray-300" />
                 <input type="text" placeholder="Address" className="w-full p-2 border border-gray-300" />
                 <input type="text" placeholder="Card Number (Demo)" className="w-full p-2 border border-gray-300" />
                 <div className="flex space-x-2">
                   <input type="text" placeholder="MM/YY" className="w-1/2 p-2 border border-gray-300" />
                   <input type="text" placeholder="CVC" className="w-1/2 p-2 border border-gray-300" />
                 </div>
               </div>
               <button 
                 onClick={handleCheckout} 
                 className="mt-4 text-black border border-black px-4 py-2 hover:bg-gray-200 transition w-full"
               >
                 Complete Purchase
               </button>
               <button 
                 onClick={() => setShowCheckout(false)} 
                 className="mt-2 text-black border border-black px-4 py-2 hover:bg-gray-200 transition w-full"
               >
                 Cancel
               </button>
             </div>
           </div>
         );
       }

       function Filters({ setCategory }) {
         return (
           <aside className="w-full sm:w-1/4 p-6">
             <h2 className="text-xl font-medium text-gray-900 mb-4">Categories</h2>
             <div className="space-y-2">
               <button onClick={() => setCategory("All")} className="block w-full text-left p-2 text-gray-700 hover:text-black">All</button>
               <button onClick={() => setCategory("Accessories")} className="block #w-full text-left p-2 text-gray-700 hover:text-black">Accessories</button>
               <button onClick={() => setCategory("Tools")} className="block w-full text-left p-2 text-gray-700 hover:text-black">Tools</button>
               <button onClick={() => setCategory("Clothing")} className="block w-full text-left p-2 text-gray-700 hover:text-black">Clothing</button>
               <button onClick={() => setCategory("Home")} className="block w-full text-left p-2 text-gray-700 hover:text-black">Home</button>
             </div>
           </aside>
         );
       }

       function App() {
         const [products] = useState(initialProducts);
         const [cart, setCart] = useState(() => JSON.parse(localStorage.getItem('cart')) || []);
         const [category, setCategory] = useState("All");
         const [searchTerm, setSearchTerm] = useState("");
         const [selectedProduct, setSelectedProduct] = useState(null);
         const [showCart, setShowCart] = useState(false);
         const [showCheckout, setShowCheckout] = useState(false);

         useEffect(() => {
           localStorage.setItem('cart', JSON.stringify(cart));
         }, [cart]);

         const addToCart = (product) => {
           if (product.stock > 0) {
             setCart([...cart, { ...product, stock: product.stock - 1 }]);
           }
         };

         const filteredProducts = products
           .filter(p => category === "All" || p.category === category)
           .filter(p => 
             p.name.toLowerCase().includes(searchTerm.toLowerCase()) || 
             p.origin.toLowerCase().includes(searchTerm.toLowerCase())
           );

         return (
           <div>
             <Header cartCount={cart.length} setSearchTerm={setSearchTerm} setShowCart={setShowCart} />
             <div className="container mx-auto flex flex-col sm:flex-row mt-8">
               <Filters setCategory={setCategory} />
               <div className="w-full sm:w-3/4 grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-8 p-6">
                 {filteredProducts.map(product => (
                   <ProductCard 
                     key={product.id} 
                     product={product} 
                     addToCart={addToCart} 
                     setSelectedProduct={setSelectedProduct} 
                   />
                 ))}
               </div>
             </div>
             <footer className="bg-black text-gray-400 p-6 text-center">
               <p>USAOnly — American Made Excellence</p>
             </footer>
             {selectedProduct && <ProductDetails product={selectedProduct} setSelectedProduct={setSelectedProduct} />}
             {showCart && <Cart cart={cart} setCart={setCart} setShowCart={setShowCart} setShowCheckout={setShowCheckout} />}
             {showCheckout && <Checkout cart={cart} setCart={setCart} setShowCheckout={setShowCheckout} />}
           </div>
         );
       }

       const root = ReactDOM.createRoot(document.getElementById('root'));
       root.render(<App />);
     </script>
   </body>
   </html>
