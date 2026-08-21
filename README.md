import 'package:flutter/material.dart';

void main() {
  runApp(const EstudaIA());
}

class EstudaIA extends StatelessWidget {
  const EstudaIA({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'EstudaIA',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const ChatPage(),
    );
  }
}

class ChatPage extends StatefulWidget {
  const ChatPage({super.key});

  @override
  State<ChatPage> createState() => _ChatPageState();
}

class _ChatPageState extends State<ChatPage> {
  final TextEditingController controller = TextEditingController();

  final List<Map<String, String>> mensagens = [
    {
      "usuario": "IA",
      "texto": "Olá! Sou a EstudaIA. Como posso ajudar nos seus estudos?"
    },
  ];

  void enviarMensagem() {
    final pergunta = controller.text.trim();

    if (pergunta.isEmpty) {
      return;
    }

    setState(() {
      mensagens.add({
        "usuario": "Aluno",
        "texto": pergunta,
      });

      mensagens.add({
        "usuario": "IA",
        "texto": responderIA(pergunta),
      });

      controller.clear();
    });
  }

  String responderIA(String pergunta) {
    final texto = pergunta.toLowerCase();

    if (texto.contains("matemática") ||
        texto.contains("matematica")) {
      return "Matemática envolve números, cálculos e raciocínio lógico. "
          "Posso ajudar com exercícios!";
    }

    if (texto.contains("história") ||
        texto.contains("historia")) {
      return "História estuda acontecimentos importantes do passado "
          "e suas consequências.";
    }

    if (texto.contains("português") ||
        texto.contains("portugues")) {
      return "Português envolve leitura, escrita, gramática "
          "e interpretação de textos.";
    }

    return "Essa é uma ótima pergunta! Vou ajudar você a aprender "
        "mais sobre esse assunto.";
  }

  @override
  void dispose() {
    controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("📚 EstudaIA"),
        centerTitle: true,
      ),
      body: Column(
        children: [
          Expanded(
            child: ListView.builder(
              padding: const EdgeInsets.all(8),
              itemCount: mensagens.length,
              itemBuilder: (context, index) {
                final mensagem = mensagens[index];

                final bool ehIA =
                    mensagem["usuario"] == "IA";

                return Container(
                  margin: const EdgeInsets.only(bottom: 8),
                  padding: const EdgeInsets.all(12),
                  decoration: BoxDecoration(
                    color: ehIA
                        ? Colors.blue[100]
                        : Colors.green[100],
                    borderRadius: BorderRadius.circular(12),
                  ),
                  child: Column(
                    crossAxisAlignment:
                        CrossAxisAlignment.start,
                    children: [
                      Text(
                        mensagem["usuario"]!,
                        style: const TextStyle(
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                      const SizedBox(height: 5),
                      Text(
                        mensagem["texto"]!,
                      ),
                    ],
                  ),
                );
              },
            ),
          ),

          Padding(
            padding: const EdgeInsets.all(8),
            child: Row(
              children: [
                Expanded(
                  child: TextField(
                    controller: controller,
                    textInputAction: TextInputAction.send,
                    onSubmitted: (_) => enviarMensagem(),
                    decoration: const InputDecoration(
                      hintText: "Digite sua dúvida...",
                      border: OutlineInputBorder(),
                    ),
                  ),
                ),

                IconButton(
                  icon: const Icon(Icons.send),
                  onPressed: enviarMensagem,
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }
}
