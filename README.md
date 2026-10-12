[README.txt](https://github.com/user-attachments/files/33337860/README.txt)# certificadosnexustreinamentos[index.html](https://github.com/user-attachments/files/33337835/index.html)
<!doctype html>
<html lang="pt-BR">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Validação de Certificados | FCC</title>
<style>
:root{--bg:#f4f6f8;--card:#fff;--text:#17202a;--muted:#667085;--line:#dfe3e8;--ok:#137333;--bad:#b3261e}
*{box-sizing:border-box}body{margin:0;background:var(--bg);font-family:Arial,Helvetica,sans-serif;color:var(--text)}
header{background:#17202a;color:white;padding:28px 20px}header .wrap,main{max-width:900px;margin:auto}
.brand{font-size:22px;font-weight:700}.tag{opacity:.8;font-size:13px;margin-top:6px}
main{padding:32px 20px}.card{background:var(--card);border:1px solid var(--line);border-radius:16px;padding:28px;box-shadow:0 4px 18px rgba(0,0,0,.05)}
h1{margin-top:0;font-size:28px}.muted{color:var(--muted)}label{display:block;font-weight:700;margin:18px 0 8px}
input{width:100%;padding:14px;border:1px solid #c9ced6;border-radius:10px;font-size:16px}
button{margin-top:14px;padding:13px 18px;border:0;border-radius:10px;background:#17202a;color:#fff;font-weight:700;cursor:pointer}
.result{margin-top:24px}.status{display:inline-block;padding:7px 11px;border-radius:999px;font-weight:700;font-size:13px;background:#e6f4ea;color:var(--ok)}
.invalid{background:#fce8e6;color:var(--bad)}.grid{display:grid;grid-template-columns:1fr 1fr;gap:14px;margin-top:18px}
.item{border-top:1px solid var(--line);padding-top:12px}.item b{display:block;font-size:12px;color:var(--muted);text-transform:uppercase;margin-bottom:4px}
.notice{margin-top:22px;padding:14px;border-left:4px solid #98a2b3;background:#f8f9fa;color:#475467;font-size:13px;line-height:1.5}
footer{text-align:center;color:var(--muted);font-size:12px;padding:30px 15px}
@media(max-width:650px){.grid{grid-template-columns:1fr}.card{padding:20px}h1{font-size:24px}}
</style>
</head>
<body>
<header><div class="wrap"><div class="brand">FCC • Certificados de Capacitação</div><div class="tag">Consulta pública de autenticidade</div></div></header>
<main>
<div class="card">
<h1>Validar certificado</h1>
<p class="muted">Digite o código do certificado ou o código de validação presente no documento.</p>
<label for="q">Código</label>
<input id="q" placeholder="Ex.: FCC-2026-00001 ou FCC-2026-00001-X7K4P9" autocomplete="off">
<button onclick="validar()">Consultar</button>
<div id="resultado" class="result"></div>
</div>
</main>
<footer>Filipe Cassemiro Camargo • Cursos livres e capacitações • Esta página comprova a autenticidade do registro informado. O eventual aproveitamento acadêmico depende da instituição de ensino.</footer>
<script>
let dados = {};
fetch('certificados.json').then(r=>r.json()).then(x=>{dados=x; carregarUrl();}).catch(()=>{document.getElementById('resultado').innerHTML='<p>Não foi possível carregar a base de certificados.</p>'});
function carregarUrl(){
 const p=new URLSearchParams(location.search); const q=p.get('codigo')||p.get('token');
 if(q){document.getElementById('q').value=q;validar();}
}
function escapar(s){return String(s??'').replace(/[&<>"']/g,m=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#039;'}[m]));}
function validar(){
 const q=document.getElementById('q').value.trim().toUpperCase();
 const r=document.getElementById('resultado');
 if(!q){r.innerHTML='<p class="muted">Informe um código.</p>';return;}
 const cert=Object.values(dados).find(x=>x.token.toUpperCase()===q || x.codigo.toUpperCase()===q);
 if(!cert){
   r.innerHTML='<div class="status invalid">CERTIFICADO NÃO LOCALIZADO</div><p>Não foi encontrado registro correspondente ao código informado.</p>';
   return;
 }
 const ok=cert.status.toUpperCase()==='VÁLIDO';
 r.innerHTML=`
 <div class="status ${ok?'':'invalid'}">${escapar(cert.status)}</div>
 <div class="grid">
 <div class="item"><b>Participante</b>${escapar(cert.participante)}</div>
 <div class="item"><b>Certificado</b>${escapar(cert.codigo)}</div>
 <div class="item"><b>Curso</b>${escapar(cert.curso)}</div>
 <div class="item"><b>Carga horária</b>${escapar(cert.carga_horaria)}</div>
 <div class="item"><b>Data</b>${escapar(cert.data)}</div>
 <div class="item"><b>Modalidade</b>${escapar(cert.modalidade)}</div>
 <div class="item"><b>Instrutor</b>${escapar(cert.instrutor)}</div>
 <div class="item"><b>Emissão</b>${escapar(cert.emissao)}</div>
 [certificados.json](https://github.com/user-attachments/files/33337844/certificados.json){
  "FCC-2026-00001-X7K4P9": {
    "codigo": "FCC-2026-00001",
    "token": "FCC-2026-00001-X7K4P9",
    "participante": "NOME DO PARTICIPANTE",
    "curso": "Ferramentas da Qualidade e Melhoria Contínua",
    "carga_horaria": "10 horas",
    "data": "05/10/2026",
    "modalidade": "Online",
    "status": "VÁLIDO",
    "instrutor": "Filipe Cassemiro Camargo",
    "emissao": "05/10/2026"
  }
}

 </div>
 [Uploading READMESISTEMA FCC — VALIDAÇÃO PÚBLICA DE CERTIFICADOS

ESTRUTURA
- index.html: página pública de consulta.
- certificados.json: banco público de registros (modelo).
- qr_demo.txt: exemplo de URL que pode ser transformada em QR Code.

COMO CADASTRAR UM CERTIFICADO
Abra certificados.json e adicione um novo objeto usando um token único.
Exemplo:
"FCC-2026-00002-A8M3Q2": {
  "codigo": "FCC-2026-00002",
  "token": "FCC-2026-00002-A8M3Q2",
  "participante": "Nome do aluno",
  "curso": "Nome do curso",
  "carga_horaria": "10 horas",
  "data": "10/10/2026",
  "modalidade": "Online",
  "status": "VÁLIDO",
  "instrutor": "Filipe Cassemiro Camargo",
  "emissao": "10/10/2026"
}

URL DE VALIDAÇÃO
Depois de publicar o site, use:
https://SEU-SITE/?codigo=FCC-2026-00001
ou
https://SEU-SITE/?token=FCC-2026-00001-X7K4P9

QR CODE
O QR Code do certificado deve apontar para uma dessas URLs.
Para cada certificado, gere um QR Code diferente.

PUBLICAÇÃO GRATUITA
Opção recomendada: GitHub Pages.
1. Crie uma conta no GitHub.
2. Crie um repositório público, por exemplo: certificados-fcc.
3. Envie index.html e certificados.json.
4. Em Settings > Pages, ative a publicação pela branch principal.
5. O GitHub fornecerá o endereço público do site.
6. Substitua a URL fictícia dos QR Codes pelo endereço real.

IMPORTANTE SOBRE PRIVACIDADE
Não coloque CPF, endereço, telefone ou outros dados desnecessários no banco público.
O banco JSON é público porque o site precisa consultá-lo.
Para uso profissional em escala, o ideal é migrar para um banco/serviço com controle de acesso e página de validação que exponha somente os dados necessários.

IMPORTANTE SOBRE A NATUREZA DO CERTIFICADO
Não use "reconhecido pelo MEC", "certificado pelo MEC" ou "válido em qualquer faculdade".
Use "curso livre de capacitação".
O aproveitamento como atividade complementar é decidido pela instituição de ensino do participante.
.txt…]()

 <div class="notice">Este registro confirma a autenticidade do certificado informado. Trata-se de curso livre/capacitação. O aproveitamento como atividade complementar acadêmica depende das regras e da análise da respectiva instituição de ensino.</div>`;
}
document.getElementById('q').addEventListener('keydown',e=>{if(e.key==='Enter')validar()});
</script>
</body>
</html>
