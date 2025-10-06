<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Mundo dos Livros</title>
<style>
body { 
  font-family: Arial, sans-serif; 
  margin: 0; 
  height: 100vh; 
  display: flex; 
  flex-direction: column; 
  overflow: hidden;
  background: #5B7DA3;
}
header { 
  background-color: #F4A261; 
  padding: 20px; 
  text-align: center; 
  font-size: 28px; 
  font-weight: bold; 
  color: #333; 
  z-index: 10;
  position: relative;
}
main { 
  flex: 1; 
  display: flex; 
  justify-content: center; 
  align-items: center; 
  position: relative;
  z-index: 5;
}
.container { 
  background: white; 
  padding: 30px; 
  border-radius: 12px; 
  box-shadow: 0px 0px 10px rgba(0,0,0,0.2); 
  width: 320px; 
  text-align: center; 
}
h2 { margin-bottom: 20px; color: #333; }
.input-group { position: relative; }
input { 
  width: 90%; padding: 10px; margin: 10px 0; 
  border: 1px solid #ccc; border-radius: 8px; 
}
.toggleSenha {
  position: absolute;
  right: 30px;
  top: 50%;
  transform: translateY(-50%);
  cursor: pointer;
  font-size: 14px;
  color: #555;
}
button { 
  background: #4CAF50; color: white; 
  border: none; padding: 10px; 
  width: 100%; border-radius: 8px; 
  cursor: pointer; transition: 0.3s; 
}
button:hover { background: #45a049; }
.erro { color: red; margin-top: 10px; font-weight: bold; }
.tabs { display: flex; justify-content: space-around; margin-bottom: 15px; }
.tabs button { background: #ddd; color: #333; flex: 1; margin: 0 5px; }
.tabs button.active { background: #4CAF50; color: white; }
.form-section { display: none; }
.form-section.active { display: block; }

/* Folhas caindo sem imagem */
.folha {
  position: fixed;
  top: -50px;
  width: 20px;
  height: 20px;
  background: orange;
  border-radius: 50% 50% 50% 0;
  opacity: 0.8;
  pointer-events: none;
  animation: cair linear infinite;
  z-index: 1;
}
@keyframes cair {
  0% { transform: translateY(0) rotate(0deg); }
  100% { transform: translateY(110vh) rotate(360deg); }
}
</style>
</head>
<body>
<header>⚜Além da imaginação⚜</header>

<main>
  <div class="container">
    <div class="tabs">
      <button id="btnLogin" class="active" onclick="mostrarForm('login')">Entrar</button>
      <button id="btnCadastro" onclick="mostrarForm('cadastro')">Cadastrar</button>
    </div>

    <div id="formLogin" class="form-section active">
      <h2>Login</h2>
      <form id="loginForm">
        <input type="email" id="loginEmail" placeholder="Digite seu e-mail" required><br>
        <div class="input-group">
          <input type="password" id="loginSenha" placeholder="Digite sua senha" required>
          <span class="toggleSenha" onclick="toggleSenha('loginSenha', this)">👁</span>
        </div>
        <button type="submit">Entrar</button>
        <div id="loginErro" class="erro"></div>
      </form>
    </div>

    <div id="formCadastro" class="form-section">
      <h2>Criar Conta</h2>
      <form id="cadastroForm">
        <input type="email" id="cadastroEmail" placeholder="Digite seu e-mail" required><br>
        <div class="input-group">
          <input type="password" id="cadastroSenha" placeholder="Crie uma senha" required>
          <span class="toggleSenha" onclick="toggleSenha('cadastroSenha', this)">👁</span>
        </div>
        <button type="submit">Cadastrar</button>
        <div id="cadastroErro" class="erro"></div>
      </form>
    </div>
  </div>
</main>

<script>
function mostrarForm(tipo) {
  document.getElementById("formLogin").classList.remove("active");
  document.getElementById("formCadastro").classList.remove("active");
  document.getElementById("btnLogin").classList.remove("active");
  document.getElementById("btnCadastro").classList.remove("active");

  if (tipo === "login") {
    document.getElementById("formLogin").classList.add("active");
    document.getElementById("btnLogin").classList.add("active");
  } else {
    document.getElementById("formCadastro").classList.add("active");
    document.getElementById("btnCadastro").classList.add("active");
  }
}

// Olhinho 👁
function toggleSenha(id, el) {
  const input = document.getElementById(id);
  if (input.type === "password") {
    input.type = "text";
    el.textContent = "🙈";
  } else {
    input.type = "password";
    el.textContent = "👁";
  }
}

// Carregar usuários
let usuarios = JSON.parse(localStorage.getItem("usuarios")) || {};
for (let email in usuarios) {
  if (typeof usuarios[email] === "string") {
    usuarios[email] = { senha: usuarios[email], dadosLivro: {} };
  }
}
localStorage.setItem("usuarios", JSON.stringify(usuarios));

// Cadastro
document.getElementById("cadastroForm").addEventListener("submit", function(e) {
  e.preventDefault();
  const email = document.getElementById("cadastroEmail").value.trim().toLowerCase();
  const senha = document.getElementById("cadastroSenha").value;

  if (usuarios[email]) {
    document.getElementById("cadastroErro").textContent = "Esse e-mail já está cadastrado!";
    return;
  }

  usuarios[email] = { senha, dadosLivro: {} };
  localStorage.setItem("usuarios", JSON.stringify(usuarios));
  document.getElementById("cadastroErro").style.color = "green";
  document.getElementById("cadastroErro").textContent = "Conta criada com sucesso! Agora faça login.";
  mostrarForm("login");
});

// Login
document.getElementById("loginForm").addEventListener("submit", function(e) {
  e.preventDefault();
  const email = document.getElementById("loginEmail").value.trim().toLowerCase();
  const senha = document.getElementById("loginSenha").value;

  if (usuarios[email] && usuarios[email].senha === senha) {
    localStorage.setItem("usuarioLogado", email);
    window.location.href = "segundou.htm"; // use o nome certo aqui
  } else {
    document.getElementById("loginErro").textContent = "E-mail ou senha incorretos!";
  }
});

// Criar folhas caindo
for (let i=0; i<20; i++) {
  const folha = document.createElement("div");
  folha.classList.add("folha");
  folha.style.left = Math.random() * 100 + "vw";
  folha.style.background = ["#d35400", "#e67e22", "#f39c12", "#c0392b"][Math.floor(Math.random()*4)];
  folha.style.animationDuration = (4 + Math.random()*6) + "s";
  folha.style.animationDelay = Math.random()*5 + "s";
  document.body.appendChild(folha);
}
</script>
</body>
</html>
