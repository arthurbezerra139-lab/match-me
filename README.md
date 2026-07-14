# match-me
void main() {
  runApp(const MatchingMeApp());
}

class MatchingMeApp extends StatelessWidget {
  const MatchingMeApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Matching Me',
      theme: ThemeData(
        useMaterial3: true,
        colorSchemeSeed: Colors.blue,
      ),
      home: const MainScreen(),
    );
  }
}

class MainScreen extends StatefulWidget {
  const MainScreen({super.key});

  @override
  State<MainScreen> createState() => _MainScreenState();
}

class _MainScreenState extends State<MainScreen> {
  int currentIndex = 0;

  final pages = const [
    DashboardScreen(),
    MatchingMeScreen(),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: pages[currentIndex],
      bottomNavigationBar: NavigationBar(
        selectedIndex: currentIndex,
        onDestinationSelected: (index) {
          setState(() {
            currentIndex = index;
          });
        },
        destinations: const [
          NavigationDestination(
            icon: Icon(Icons.dashboard),
            label: 'Dashboard',
          ),
          NavigationDestination(
            icon: Icon(Icons.people),
            label: 'Matching Me',
          ),
        ],
      ),
    );
  }
}

class DashboardScreen extends StatelessWidget {
  const DashboardScreen({super.key});

  final List<Map<String, dynamic>> tasks = const [
    {
      "titulo": "Automação Power Automate",
      "status": "Em andamento"
    },
    {
      "titulo": "Dashboard Power BI",
      "status": "Revisão"
    },
    {
      "titulo": "Integração SAP",
      "status": "Não iniciada"
    },
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Matching Me - Dashboard"),
      ),
      body: ListView.builder(
        itemCount: tasks.length,
        itemBuilder: (context, index) {
          return Card(
            margin: const EdgeInsets.all(8),
            child: ListTile(
              leading: const Icon(Icons.task),
              title: Text(tasks[index]["titulo"]),
              subtitle: Text(tasks[index]["status"]),
            ),
          );
        },
      ),
    );
  }
}

class MatchingMeScreen extends StatefulWidget {
  const MatchingMeScreen({super.key});

  @override
  State<MatchingMeScreen> createState() =>
      _MatchingMeScreenState();
}

class _MatchingMeScreenState
    extends State<MatchingMeScreen> {
  final TextEditingController controller =
      TextEditingController();

  final List<String> mensagens = [];

  final List<Map<String, dynamic>> funcionarios = [
    {
      "nome": "Carlos Silva",
      "especializacao": "SAP, Power Automate"
    },
    {
      "nome": "Ana Souza",
      "especializacao": "Power BI, Excel"
    }
  ];

  void processarMatch() {
    String texto = controller.text;

    if (texto.isEmpty) return;

    setState(() {
      mensagens.add("Funcionário: $texto");

      if (texto.toLowerCase().contains("sap")) {
        mensagens.add(
            "✅ Match encontrado: Carlos Silva - Especialista SAP");
      } else if (texto
          .toLowerCase()
          .contains("power bi")) {
        mensagens.add(
            "✅ Match encontrado: Ana Souza - Especialista Power BI");
      } else {
        mensagens.add(
            "⚠ Nenhum especialista encontrado.");
      }

      controller.clear();
    });
  }

  void adicionarFuncionario() {
    showDialog(
      context: context,
      builder: (context) {
        TextEditingController nome =
            TextEditingController();

        TextEditingController especializacao =
            TextEditingController();

        return AlertDialog(
          title: const Text("Novo Funcionário"),
          content: Column(
            mainAxisSize: MainAxisSize.min,
            children: [
              TextField(
                controller: nome,
                decoration: const InputDecoration(
                  labelText: "Nome",
                ),
              ),
              TextField(
                controller: especializacao,
                decoration: const InputDecoration(
                  labelText: "Especializações",
                ),
              ),
            ],
          ),
          actions: [
            ElevatedButton(
              onPressed: () {
                setState(() {
                  funcionarios.add({
                    "nome": nome.text,
                    "especializacao":
                        especializacao.text
                  });
                });

                Navigator.pop(context);
              },
              child: const Text("Salvar"),
            )
          ],
        );
      },
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Matching Me"),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: adicionarFuncionario,
        child: const Icon(Icons.person_add),
      ),
      body: Column(
        children: [
          const SizedBox(height: 10),

          const Text(
            "IA de Match de Especialistas",
            style: TextStyle(
              fontSize: 20,
              fontWeight: FontWeight.bold,
            ),
          ),

          Expanded(
            child: ListView.builder(
              itemCount: mensagens.length,
              itemBuilder: (context, index) {
                return ListTile(
                  title: Text(mensagens[index]),
                );
              },
            ),
          ),

          Padding(
            padding: const EdgeInsets.all(12),
            child: Row(
              children: [
                Expanded(
                  child: TextField(
                    controller: controller,
                    decoration:
                        const InputDecoration(
                      hintText:
                          "Descreva sua dificuldade...",
                    ),
                  ),
                ),
                IconButton(
                  icon: const Icon(Icons.send),
                  onPressed: processarMatch,
                )
              ],
            ),
          ),

          const Divider(),

          const Padding(
            padding: EdgeInsets.all(8),
            child: Text(
              "Funcionários Cadastrados",
              style: TextStyle(
                fontWeight: FontWeight.bold,
              ),
            ),
          ),

          SizedBox(
            height: 200,
            child: ListView.builder(
              itemCount: funcionarios.length,
              itemBuilder: (context, index) {
                return Card(
                  child: ListTile(
                    leading:
                        const CircleAvatar(
                      child: Icon(Icons.person),
                    ),
                    title: Text(
                        funcionarios[index]["nome"]),
                    subtitle: Text(
                        funcionarios[index]
                            ["especializacao"]),
                  ),
                );
              },
            ),
          ),
        ],
      ),
    );
  }
}