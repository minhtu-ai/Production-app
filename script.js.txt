import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.0/firebase-app.js";
import { getFirestore, collection, addDoc, onSnapshot } from "https://www.gstatic.com/firebasejs/10.7.0/firebase-firestore.js";

const firebaseConfig = {
  // DÁN C?U HÌNH FIREBASE C?A B?N VÀO ÐÂY
};

const app = initializeApp(firebaseConfig);
const db = getFirestore(app);

window.saveData = async function () {
  const product = document.getElementById("product").value;
  const worker = document.getElementById("worker").value;
  const material = document.getElementById("material").value;
  const batch = document.getElementById("batch").value;
  const quantity = document.getElementById("quantity").value;

  await addDoc(collection(db, "production"), {
    product,
    worker,
    material,
    batch,
    quantity,
    createdAt: new Date()
  });
};

onSnapshot(collection(db, "production"), (snapshot) => {
  const list = document.getElementById("list");
  list.innerHTML = "";
  snapshot.forEach(doc => {
    const data = doc.data();
    const li = document.createElement("li");
    li.textContent = data.product + " - " + data.quantity;
    list.appendChild(li);
  });
});