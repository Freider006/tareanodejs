const express = require("express");
const cors = require("cors");

const app = express();
app.use(cors());
app.use(express.json());

let electrodomesticos = [
    { id: "1", nombre: "Refrigerador", descripcion: "Capacidad 300L, No Frost", precio: 1200 },
    { id: "2", nombre: "Microondas", descripcion: "1000W con grill", precio: 350 },
    { id: "3", nombre: "Licuadora", descripcion: "Motor de 600W, 5 velocidades", precio: 180 },
];

app.get("/", (req, res) => {
    res.send("Servidor 3000");
});

app.get("/lista", (req, res) => {
    res.json(electrodomesticos);
});

app.post("/lista", (req, res) => {
    const { id, nombre, descripcion, precio } = req.body;
    if (!id || !nombre || !descripcion || !precio) {
        return res.json({ mensaje: "Mal ingresado" });
    }
    electrodomesticos.push({ id, nombre, descripcion, precio });
    return res.json({ mensaje: "Electrodoméstico agregado correctamente" });
});

app.put("/lista/:id", (req, res) => {
    const { id } = req.params;
    const { nombre, descripcion, precio } = req.body;
    const electrodomestico = electrodomesticos.find((p) => p.id === id);
    if (!electrodomestico) {
        return res.status(400).json({ mensaje: "Electrodoméstico no encontrado" });
    }
    if (nombre) { electrodomestico.nombre = nombre; }
    if (descripcion) { electrodomestico.descripcion = descripcion; }
    if (precio) { electrodomestico.precio = precio; }
    return res.json({ mensaje: "Electrodoméstico actualizado correctamente" });
});

app.delete("/lista/:id", (req, res) => {
    const { id } = req.params;
    electrodomesticos = electrodomesticos.filter((p) => p.id !== id);
    res.json({ mensaje: "Electrodoméstico eliminado" });
});

app.listen(3000, () => {
    console.log("Servidor corriendo en el puerto 3000");
});
