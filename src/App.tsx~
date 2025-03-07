import React, { useState, useEffect, useRef } from 'react';
import JsBarcode from 'jsbarcode';
import { Barcode, Lock, User, UserCircle, Eye, EyeOff } from 'lucide-react';

function App() {
  const [fullName, setFullName] = useState('');
  const [generatedUsername, setGeneratedUsername] = useState('');
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');
  const [showPassword, setShowPassword] = useState(false); // Controla la visibilidad de la contraseña

  const usernameBarcodeRef = useRef<HTMLCanvasElement>(null);
  const passwordBarcodeRef = useRef<HTMLCanvasElement>(null);

  // Función para generar nombre de usuario sugerido
  const generateUsername = (name: string) => {
    if (!name.trim()) return '';

    // Remove accents and convert to lowercase
    const normalized = name.normalize("NFD").replace(/[\u0300-\u036f]/g, "").toLowerCase();

    // Split the name into parts
    const parts = normalized.split(' ').filter(part => part.length > 0);
    if (parts.length < 2) return '';

    // Get first letter of the first name
    const firstLetter = parts[0].charAt(0);

    // Get the second to last word (as paternal surname)
    const paternalSurname = parts[parts.length - 2];

    return `${firstLetter}${paternalSurname}`;
  };

  // Función para descargar la imagen generada en el <canvas>
  const descargarCodigo = (canvasRef: React.RefObject<HTMLCanvasElement>, filename: string) => {
    if (canvasRef.current) {
      const link = document.createElement('a');
      link.download = `${filename}.png`;
      link.href = canvasRef.current.toDataURL('image/png');
      link.click();
    }
  };

  // Actualiza el nombre de usuario sugerido al cambiar el nombre completo
  useEffect(() => {
    const newUsername = generateUsername(fullName);
    setGeneratedUsername(newUsername);
  }, [fullName]);

  // Generar código de barras para "username"
  useEffect(() => {
    if (username && usernameBarcodeRef.current) {
      JsBarcode(usernameBarcodeRef.current, username, {
        format: "CODE128",
        width: 2,
        height: 100,
        displayValue: false,  // ocultar el texto
        background: "#ffffff",
      });
    }
  }, [username]);

  // Generar código de barras para "password"
  useEffect(() => {
    if (password && passwordBarcodeRef.current) {
      JsBarcode(passwordBarcodeRef.current, password, {
        format: "CODE128",
        width: 2,
        height: 100,
        displayValue: false, // ocultar el texto
        background: "#ffffff",
      });
    }
  }, [password]);

  // Toggle para mostrar/ocultar la contraseña
  const toggleShowPassword = () => {
    setShowPassword((prev) => !prev);
  };

  return (
      <div className="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100 p-8">
        <div className="max-w-2xl mx-auto">
          <div className="bg-white rounded-xl shadow-xl p-8">
            <div className="flex items-center gap-3 mb-8">
              <Barcode className="w-8 h-8 text-indigo-600" />
              <h1 className="text-3xl font-bold text-gray-800">Generador de Códigos de Barra</h1>
            </div>

            <div className="space-y-6">
              {/* Nombre Completo */}
              <div>
                <div className="flex items-center gap-2 mb-2">
                  <UserCircle className="w-5 h-5 text-gray-600" />
                  <label className="block text-sm font-medium text-gray-700">
                    Nombre Completo
                  </label>
                </div>
                <input
                    type="text"
                    value={fullName}
                    onChange={(e) => setFullName(e.target.value)}
                    className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
                    placeholder="Ingrese su nombre completo (Nombre Apellido Paterno Apellido Materno)"
                />
                {generatedUsername && (
                    <div className="mt-2 text-sm text-gray-600">
                      Nombre de usuario sugerido: <span className="font-medium">{generatedUsername}</span>
                    </div>
                )}
              </div>

              {/* Nombre de Usuario */}
              <div>
                <div className="flex items-center gap-2 mb-2">
                  <User className="w-5 h-5 text-gray-600" />
                  <label className="block text-sm font-medium text-gray-700">
                    Nombre de Usuario
                  </label>
                </div>
                <input
                    type="text"
                    value={username}
                    onChange={(e) => setUsername(e.target.value)}
                    className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
                    placeholder="Ingrese su nombre de usuario"
                />
                {username && (
                    <div className="mt-4 p-4 bg-gray-50 rounded-lg flex flex-col items-center">
                      <canvas ref={usernameBarcodeRef}></canvas>
                      <button
                          onClick={() => descargarCodigo(usernameBarcodeRef, 'barcode-username')}
                          className="mt-2 px-4 py-2 bg-green-500 text-white rounded hover:bg-green-600"
                      >
                        Descargar Código
                      </button>
                    </div>
                )}
              </div>

              {/* Contraseña */}
              <div>
                <div className="flex items-center gap-2 mb-2">
                  <Lock className="w-5 h-5 text-gray-600" />
                  <label className="block text-sm font-medium text-gray-700">
                    Contraseña
                  </label>
                </div>
                <div className="relative">
                  <input
                      type={showPassword ? 'text' : 'password'}
                      value={password}
                      onChange={(e) => setPassword(e.target.value)}
                      className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500 pr-10"
                      placeholder="Ingrese su contraseña"
                  />
                  <button
                      type="button"
                      onClick={toggleShowPassword}
                      className="absolute right-2 top-2 text-gray-500 hover:text-gray-700"
                  >
                    {showPassword ? <EyeOff className="w-5 h-5" /> : <Eye className="w-5 h-5" />}
                  </button>
                </div>
                {password && (
                    <div className="mt-4 p-4 bg-gray-50 rounded-lg flex flex-col items-center">
                      <canvas ref={passwordBarcodeRef}></canvas>
                      <button
                          onClick={() => descargarCodigo(passwordBarcodeRef, 'barcode-password')}
                          className="mt-2 px-4 py-2 bg-green-500 text-white rounded hover:bg-green-600"
                      >
                        Descargar Código
                      </button>
                    </div>
                )}
              </div>
            </div>

            <p className="mt-6 text-sm text-gray-600 text-center">
              Los códigos de barra se generarán automáticamente al ingresar los datos
            </p>
          </div>
        </div>
      </div>
  );
}

export default App;
