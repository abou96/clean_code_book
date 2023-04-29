# Principes du prompting

## Principe 1: Écrire des instructions claires et précises

### Tactique 1: Utilisez des délimiteurs
Les délimiteurs sont des caractères spéciaux ou des balises qui indiquent le début et la fin d'une information ou de données. Quelques exemples de délimiteurs sont:

- Triple guillemets: ''' '''
- Triple backticks: ``````
- Triple tirets: ---
- Chevrons: <>
- Balises XML: <tag> </tag> 

Par exemple, lorsque vous fournissez une phrase en entrée à un modèle de langage, vous pouvez utiliser des triples backticks comme délimiteurs pour indiquer le début et la fin de la phrase, comme ceci :

```python

prompt = f"""
Summarize the text delimited by triple backticks \ 
into a single sentence.
```{your text}```
"""

```

### Tactique 2: Demander une sortie structurée (HTML, JSON)

Les formats de sortie structurés tels que HTML ou JSON fournissent une façon claire et organisée de représenter des données. Demander une sortie structurée peut aider le modèle à comprendre le type de données attendues en sortie et à les fournir dans un format plus organisé et facilement analysable. Par exemple, lorsqu'on demande à un modèle de générer une liste d'articles, on peut lui demander de fournir les articles sous forme de JSON :

```python

prompt = f"""
Generate a list of three made-up book titles along \ 
with their authors and genres. 
Provide them in JSON format with the following keys: 
book_id, title, author, genre.
"""
```

### Tactique 3: Vérifier si les conditions sont remplies. Vérifier les hypothèses nécessaires pour effectuer la tâche.

Avant de demander au modèle d'effectuer une tâche, il est important de vérifier si les conditions nécessaires sont remplies et si des hypothèses doivent être faites.


```python

prompt = f"""
You will be provided with text delimited by triple quotes. 
If it contains a sequence of instructions, \ 
re-write those instructions in the following format:

Step 1 - ...
Step 2 - …
…
Step N - …

If the text does not contain a sequence of instructions, \ 
then simply write \"No steps provided.\"

\"\"\"{text_1}\"\"\"
"""
```

### Tactique 4: "Few-shot prompting"

Le "Few-shot prompting" consiste à fournir au modèle quelques exemples réussis de l'accomplissement d'une tâche, puis à lui demander d'effectuer la tâche. Cette approche peut aider le modèle à apprendre à effectuer une nouvelle tâche plus rapidement et avec plus de précision.

```python

prompt = f"""
Your task is to answer in a consistent style.

<child>: Teach me about patience.

<grandparent>: The river that carves the deepest \ 
valley flows from a modest spring; the \ 
grandest symphony originates from a single note; \ 
the most intricate tapestry begins with a solitary thread.

<child>: Teach me about resilience.
"""

```

## Principe 2 : Donner au modèle le temps de réfléchir

### Tactique 1 : Spécifier les étapes pour accomplir une tâche

Lorsqu'on demande à un modèle d'accomplir une tâche, il peut être utile de spécifier les étapes impliquées dans l'accomplissement de la tâche. Cela peut aider le modèle à comprendre la structure de la tâche et fournir une réponse plus précise.

```python
prompt = f"""
Perform the following actions: 
1 - Summarize the following text delimited by triple \
backticks with 1 sentence.
2 - Translate the summary into French.
3 - List each name in the French summary.
4 - Output a json object that contains the following \
keys: french_summary, num_names.

Separate your answers with line breaks.

Text:
```{text}```
"""
```

### Tactique 2 : Instruire le modèle pour qu'il trouve sa propre solution avant de se précipiter vers une conclusion

Lorsqu'on demande à un modèle de résoudre un problème ou d'accomplir une tâche, il est important de permettre au modèle de trouver sa propre solution plutôt que de se précipiter vers une conclusion. Cela peut aider le modèle à mieux comprendre le problème et à fournir une réponse plus précise. Par exemple, lorsqu'on demande à un modèle de résoudre un problème mathématique, on pourrait donner les instructions suivantes :

```python
prompt = f"""
Your task is to determine if the student's solution \
is correct or not.
To solve the problem do the following:
- First, work out your own solution to the problem. 
- Then compare your solution to the student's solution \ 
and evaluate if the student's solution is correct or not. 
Don't decide if the student's solution is correct until 
you have done the problem yourself.

Use the following format:
Question:
```
question here
```
Student's solution:
```
student's solution here
```
Actual solution:
```
steps to work out the solution and your solution here
```
Is the student's solution the same as actual solution \
just calculated:
```
yes or no
```
Student grade:
```
correct or incorrect
```

Question:
```
I'm building a solar power installation and I need help \
working out the financials. 
- Land costs $100 / square foot
- I can buy solar panels for $250 / square foot
- I negotiated a contract for maintenance that will cost \
me a flat $100k per year, and an additional $10 / square \
foot
What is the total cost for the first year of operations \
as a function of the number of square feet.
``` 
Student's solution:
```
Let x be the size of the installation in square feet.
Costs:
1. Land cost: 100x
2. Solar panel cost: 250x
3. Maintenance cost: 100,000 + 100x
Total cost: 100x + 250x + 100,000 + 100x = 450x + 100,000
```
Actual solution:
"""
```


