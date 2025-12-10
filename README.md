<!doctype html>
<html lang="pt-BR">

<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>PUC-SP — Sistema de Verificação de Diplomas</title>

    <style>
        :root {
            --bg: #f4f6fa;
            --card: #ffffff;
            --accent: #003366; /* Azul oficial PUC-SP */
            --muted: #6b7280;
            --gold: #d4af37; /* Dourado institucional */
        }

        body {
            font-family: Inter, system-ui, -apple-system, 'Segoe UI', Roboto, Arial;
            background: var(--bg);
            margin: 0;
            color: #0f172a;
        }

        header {
            background: var(--accent);
            padding: 24px 28px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            border-bottom: 4px solid var(--gold);
        }

        .brand {
            font-weight: 700;
            font-size: 18px;
            color: white;
        }

        .subtitle {
            color: #dfe7f2;
            font-size: 14px;
        }

        .container {
            max-width: 980px;
            margin: 36px auto;
            padding: 20px;
        }

        .search {
            background: var(--card);
            border-radius: 12px;
            padding: 20px;
            box-shadow: 0 6px 18px rgba(0, 0, 0, 0.06);
            display: flex;
            gap: 16px;
            align-items: center;
        }

        .search input {
            flex: 1;
            padding: 14px 16px;
            border: 1px solid #d0d6df;
            border-radius: 8px;
            font-size: 15px;
        }

        .search button {
            background: var(--accent);
            color: white;
            border: none;
            padding: 12px 18px;
            border-radius: 8px;
            font-weight: 600;
            cursor: pointer;
        }

        .search button:hover {
            background: #00284f;
        }

        .content {
            display: flex;
            gap: 20px;
            margin-top: 20px;
        }

        .left {
            flex: 1;
        }

        .right {
            width: 460px;
        }

        .card {
            background: var(--card);
            border-radius: 12px;
            padding: 22px;
            box-shadow: 0 6px 18px rgba(0, 0, 0, 0.06);
        }

        .muted {
            color: var(--muted);
            font-size: 14px;
        }

        .meta {
            margin-top: 12px;
            font-size: 15px;
        }

        .label {
            display: inline-block;
            padding: 6px 12px;
            border-radius: 8px;
            background: var(--accent);
            color: white;
            font-weight: 700;
        }

        .img-wrap {
            background: #f3f6fd;
            padding: 18px;
            border-radius: 10px;
            border: 1px solid #dce4f7;
        }

        #previewCanvas {
            max-width: 100%;
            border-radius: 8px;
            background: white;
        }

        footer {
            max-width: 980px;
            margin: 28px auto;
            color: var(--muted);
            font-size: 13px;
            text-align: center;
        }

        @media (max-width:820px) {
            .content {
                flex-direction: column;
            }

            .right {
                width: 100%;
            }
        }
    </style>
</head>

<body>

    <header>
        <div class="brand" id="brand">PUC-SP — Sistema de Verificação de Diplomas</div>
        <div class="subtitle" id="subtitle">Pontifícia Universidade Católica de São Paulo</div>
    </header>

    <div class="container">

        <div class="search">
            <input id="q" placeholder="Consultar por número do diploma (ex: 57656-86)" />
            <button id="btn">Consultar</button>
        </div>

        <div class="content">

            <div class="left">
                <div class="card" id="result">
                    <div class="muted">Resultado da verificação</div>

                    <h2 id="name">—</h2>
                    <div class="meta" id="degree">—</div>
                    <div class="meta" id="inst">—</div>
                    <div class="meta" id="date">—</div>

                    <div style="margin-top:14px;">
                        <span class="label" id="status">—</span>
                    </div>

                    <hr style="margin:18px 0;border:none;border-top:1px solid #eef2f6" />

                    <div class="muted">Número do diploma</div>
                    <div id="dnum" style="font-weight:700;margin-top:6px">—</div>
                </div>
            </div>

            <div class="right">
                <div class="card">
                    <div class="muted">Visualização</div>
                    <div class="img-wrap" style="margin-top:12px;display:flex;justify-content:center">
                        <canvas id="previewCanvas" width="420" height="260"></canvas>
                    </div>
                </div>
            </div>

        </div>

    </div>

    <footer id="footerText">
        PUC-SP — © 2025
    </footer>

    <script>
        const records = {
            '577546-75': {
                diploma: '577546-75',
                name: 'Franciele Ferreira de Melo S. Siqueira',
                degree: 'Bacharel em Fisioterapia',
                inst: 'Pontifícia Universidade Católica de Campinas (PUC-Campinas)',
                date: 'Conclusão em 10/08/2024',
                status: 'Ativo'
            },
            '47757-86': {
                diploma: '47757-86',
                name: 'Alex Aparecido Fernandes Rosseti',
                degree: 'Mestrado em Psicologia',
                inst: 'Pontifícia Universidade Católica de São Paulo (PUC-SP)',
                date: 'Conclusão em 14/08/2022',
                status: 'Ativo'
            },
            '57656-86': {
                diploma: '57656-86',
                name: 'Alex Aparecido Fernandes Rosseti',
                degree: 'Doutorado em Psicologia',
                inst: 'Pontifícia Universidade Católica de São Paulo (PUC-SP)',
                date: 'Conclusão em 24/06/2025',
                status: 'Ativo'
            },
            '476566-87': {
                diploma: '476566-87',
                name: 'Nicole de Souza Queiroz',
                degree: 'Licenciatura em Música',
                inst: 'Pontifícia Universidade Católica do Rio Grande do Sul (PUC-RS)',
                date: 'Conclusão em 10/08/2025',
                status: 'Ativo'
            },
            '475557-86': {
                diploma: '475557-86',
                name: 'Itamara Cássia Moreira do Patrocínio',
                degree: 'Bacharel em Terapia Ocupacional',
                inst: 'Pontifícia Universidade Católica de Minas Gerais (PUC-Minas)',
                date: 'Conclusão em 29/08/2023',
                status: 'Ativo'
            },

            /* --- NOVO REGISTRO: JÚLIO CEZAR --- */
            '35644-76': {
                diploma: '35644-76',
                name: 'Julio Cezar de Oliveira',
                degree: 'Mestrado em Educação Matemática',
                inst: 'Pontifícia Universidade Católica do Paraná (PUC-PR)',
                date: 'Conclusão em 20/08/2019',
                status: 'Ativo'
            }
        };

        document.getElementById('btn').addEventListener('click', function () {
            const q = document.getElementById('q').value.trim();
            const result = records[q];
            const brand = document.getElementById('brand');
            const footerText = document.getElementById('footerText');

            if (result) {
                document.getElementById('name').innerText = result.name;
                document.getElementById('degree').innerText = result.degree;
                document.getElementById('inst').innerText = result.inst;
                document.getElementById('date').innerText = result.date;
                document.getElementById('status').innerText = result.status;
                document.getElementById('dnum').innerText = result.diploma;

                brand.innerText = `${result.inst} — Sistema de Verificação de Diplomas`;
                footerText.innerText = `${result.inst} — © 2025`;
            } else {
                alert('Diploma não encontrado. Verifique o número digitado.');

                document.getElementById('name').innerText = '—';
                document.getElementById('degree').innerText = '—';
                document.getElementById('inst').innerText = '—';
                document.getElementById('date').innerText = '—';
                document.getElementById('status').innerText = '—';
                document.getElementById('dnum').innerText = '—';

                brand.innerText = `PUC-SP — Sistema de Verificação de Diplomas`;
                footerText.innerText = `PUC-SP — © 2025`;
            }
        });
    </script>

</body>

</html>
