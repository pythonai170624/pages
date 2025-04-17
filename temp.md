# Supervised Machine Learning Algorithms - Decision Trees

## Last Lecture Reminder
We learned about:
- SVM - Kernel trick computation
- SVM Classification - Python example
- SVM Classification - Linear Kernel
- SVM Classification - RBF Kernel 
- SVM Classification - Polynomial Kernel 
- SVM Classification - Grid Search
- SVR (Support Vector Regression) - SVM model for Continuous data

## Decision Trees - Theory
Decision Trees algorithms → Decision Trees in Supervised Learning are algorithms that mimic the logical process of human decision-making, applied to predict an output given the provided inputs. In a decision tree, the internal nodes represent the features of a data set, the branches represent the decision rules, and each leaf node represents the outcome.

Conceptually, it can be considered as dividing a large set of conditions/data into smaller ones, which then further branch into even smaller sets, forming a tree-like structure.

The primary advantage of Decision Trees is their ease of interpretation and visualization, while their major disadvantage lies in their tendency to overfit data, low prediction accuracy and bias in handling imbalanced datasets.

## Decision Tree - Terminology

Before starting to explore the different decision tree algorithms we first need to understand the decision tree basic terminology.

For that, let's take a simple following decision tree (X - feature value, Y - label value):

```
              ┌─────────┐
              │  X ≤ 1  │
              └────┬────┘
                   │
        ┌──────────┴──────────┐
        │                     │
        ▼                     ▼
   ┌────────┐            ┌────────┐
   │ Y=1.5  │            │  X ≤ 2 │
   └────────┘            └────┬───┘
                              │
                    ┌─────────┴─────────┐
                    │                   │
                    ▼                   ▼
               ┌────────┐          ┌────────┐
               │ Y=2.2  │          │ Y=1.5  │
               └────────┘          └────────┘
```

- If X ≤ 1: The label outcome will be 1.5
- If X > 1 and X ≤ 2: The label outcome will be 2.2
- If X > 1 and X > 2: The label outcome will be 1.5

Let's now understand some basic terminologies regarding decision trees and trees in general:

- **Node** → A node is an entity that contains a value or data and can be connected by links to other nodes.
- **Parent and child** → A node, which is divided into sub-nodes is called parent of sub-nodes whereas the sub-nodes are the children of a parent node.
- **Root** → The top node in a tree, with no parent meaning no nodes that are above him, the root is the first node in the tree and all the other nodes are it's children.
- **Leaf** → A node which does not have any child node is called a leaf node.
- **Edge** → The connection between one node and another.
- **Height of a Node** → The length of the longest downward path to a leaf from that node.
- **Depth of a Node** → The length of the path to its root.
- **Sub-tree** → Subtree represents the descendants of a node.

<svg viewBox="0 0 600 300" xmlns="http://www.w3.org/2000/svg">
  <!-- Tree Nodes Example -->
  <rect x="50" y="20" width="100" height="60" fill="#f2d388" stroke="#000" rx="10" ry="10"/>
  <text x="100" y="55" font-size="16" text-anchor="middle">X ≤ 1</text>
  
  <line x1="100" y1="80" x2="50" y2="130" stroke="#000" stroke-width="2"/>
  <text x="65" y="115" font-size="12">T</text>
  
  <line x1="100" y1="80" x2="150" y2="130" stroke="#000" stroke-width="2"/>
  <text x="135" y="115" font-size="12">F</text>
  
  <rect x="10" y="130" width="80" height="60" fill="#f2d388" stroke="#000" rx="10" ry="10"/>
  <text x="50" y="165" font-size="16" text-anchor="middle">Y=1.5</text>
  
  <rect x="110" y="130" width="80" height="60" fill="#f2d388" stroke="#000" rx="10" ry="10"/>
  <text x="150" y="165" font-size="16" text-anchor="middle">X ≤ 2</text>
  
  <line x1="150" y1="190" x2="120" y2="240" stroke="#000" stroke-width="2"/>
  <text x="130" y="225" font-size="12">T</text>
  
  <line x1="150" y1="190" x2="180" y2="240" stroke="#000" stroke-width="2"/>
  <text x="170" y="225" font-size="12">F</text>
  
  <rect x="80" y="240" width="80" height="60" fill="#f2d388" stroke="#000" rx="10" ry="10"/>
  <text x="120" y="275" font-size="16" text-anchor="middle">Y=2.2</text>
  
  <rect x="140" y="240" width="80" height="60" fill="#f2d388" stroke="#000" rx="10" ry="10"/>
  <text x="180" y="275" font-size="16" text-anchor="middle">Y=1.5</text>
  
  <!-- Tree Terminology -->
  <rect x="280" y="40" width="300" height="30" fill="none" stroke="#000"/>
  <text x="430" y="60" font-size="14" text-anchor="middle">Tree nodes</text>
  <line x1="320" y1="70" x2="50" y2="165" stroke="#f00" stroke-width="2"/>
  
  <rect x="280" y="100" width="300" height="30" fill="none" stroke="#000"/>
  <text x="430" y="120" font-size="14" text-anchor="middle">Tree Root node</text>
  <line x1="320" y1="130" x2="100" y2="55" stroke="#f00" stroke-width="2"/>
  
  <rect x="280" y="160" width="300" height="30" fill="none" stroke="#000"/>
  <text x="430" y="180" font-size="14" text-anchor="middle">Tree Leaf nodes</text>
  <line x1="320" y1="190" x2="120" y2="275" stroke="#f00" stroke-width="2"/>
  
  <rect x="280" y="220" width="300" height="30" fill="none" stroke="#000"/>
  <text x="430" y="240" font-size="14" text-anchor="middle">Edge</text>
  <line x1="320" y1="250" x2="150" y2="150" stroke="#f00" stroke-width="2"/>
</svg>

<svg viewBox="0 0 600 300" xmlns="http://www.w3.org/2000/svg">
  <!-- Parent-Child and Subtree Relationships -->
  <rect x="50" y="20" width="100" height="60" fill="#c8e6c9" stroke="#000" rx="10" ry="10"/>
  <text x="100" y="55" font-size="16" text-anchor="middle">X ≤ 1</text>
  
  <line x1="100" y1="80" x2="50" y2="130" stroke="#000" stroke-width="2"/>
  <text x="65" y="115" font-size="12">T</text>
  
  <line x1="100" y1="80" x2="150" y2="130" stroke="#000" stroke-width="2"/>
  <text x="135" y="115" font-size="12">F</text>
  
  <rect x="10" y="130" width="80" height="60" fill="#ffccbc" stroke="#000" rx="10" ry="10"/>
  <text x="50" y="165" font-size="16" text-anchor="middle">Y=1.5</text>
  
  <rect x="110" y="130" width="80" height="60" fill="#d1c4e9" stroke="#000" rx="10" ry="10"/>
  <text x="150" y="165" font-size="16" text-anchor="middle">X ≤ 2</text>
  
  <line x1="150" y1="190" x2="120" y2="240" stroke="#000" stroke-width="2"/>
  <text x="130" y="225" font-size="12">T</text>
  
  <line x1="150" y1="190" x2="180" y2="240" stroke="#000" stroke-width="2"/>
  <text x="170" y="225" font-size="12">F</text>
  
  <rect x="80" y="240" width="80" height="60" fill="#ffe0b2" stroke="#000" rx="10" ry="10"/>
  <text x="120" y="275" font-size="16" text-anchor="middle">Y=2.2</text>
  
  <rect x="140" y="240" width="80" height="60" fill="#ffe0b2" stroke="#000" rx="10" ry="10"/>
  <text x="180" y="275" font-size="16" text-anchor="middle">Y=1.5</text>
  
  <!-- Relationship Labels -->
  <rect x="280" y="40" width="300" height="30" fill="none" stroke="#000"/>
  <text x="430" y="60" font-size="14" text-anchor="middle">Parent node for y=1.5 and x<= 2</text>
  <line x1="320" y1="70" x2="100" y2="55" stroke="#f00" stroke-width="2"/>
  
  <rect x="280" y="100" width="300" height="30" fill="none" stroke="#000"/>
  <text x="430" y="120" font-size="14" text-anchor="middle">Children nodes of x<= 2 node</text>
  <line x1="320" y1="130" x2="120" y2="275" stroke="#f00" stroke-width="2"/>
  
  <rect x="280" y="160" width="300" height="30" fill="none" stroke="#000"/>
  <text x="430" y="180" font-size="14" text-anchor="middle">Subtree nodes</text>
  <line x1="320" y1="190" x2="150" y2="165" stroke="#f00" stroke-width="2"/>
</svg>

## Tree Depth and Height

- **Tree depth** → The depth of a tree could be seen as the depth of the deepest node in the tree, or the maximum level any node is on. The root node is the first level, its immediate child nodes are on the second level and so on.
- **Tree height** → The height of a tree is taken as the height of the root node, and is the length of the longest path from the root node to any leaf node in the tree.

<svg viewBox="0 0 600 300" xmlns="http://www.w3.org/2000/svg">
  <!-- Tree with Depth and Height Labels -->
  <rect x="250" y="20" width="100" height="60" fill="#ffcdd2" stroke="#000" rx="10" ry="10"/>
  <text x="300" y="55" font-size="16" text-anchor="middle">X ≤ 1</text>
  
  <line x1="300" y1="80" x2="200" y2="130" stroke="#000" stroke-width="2"/>
  <text x="240" y="115" font-size="12">T</text>
  
  <line x1="300" y1="80" x2="400" y2="130" stroke="#000" stroke-width="2"/>
  <text x="360" y="115" font-size="12">F</text>
  
  <rect x="160" y="130" width="80" height="60" fill="#c8e6c9" stroke="#000" rx="10" ry="10"/>
  <text x="200" y="165" font-size="16" text-anchor="middle">Y=1.5</text>
  
  <rect x="360" y="130" width="80" height="60" fill="#ffe0b2" stroke="#000" rx="10" ry="10"/>
  <text x="400" y="165" font-size="16" text-anchor="middle">X ≤ 2</text>
  
  <line x1="400" y1="190" x2="350" y2="240" stroke="#000" stroke-width="2"/>
  <text x="370" y="225" font-size="12">T</text>
  
  <line x1="400" y1="190" x2="450" y2="240" stroke="#000" stroke-width="2"/>
  <text x="430" y="225" font-size="12">F</text>
  
  <rect x="310" y="240" width="80" height="60" fill="#bbdefb" stroke="#000" rx="10" ry="10"/>
  <text x="350" y="275" font-size="16" text-anchor="middle">Y=2.2</text>
  
  <rect x="410" y="240" width="80" height="60" fill="#d1c4e9" stroke="#000" rx="10" ry="10"/>
  <text x="450" y="275" font-size="16" text-anchor="middle">Y=1.5</text>
  
  <!-- Depth Labels -->
  <rect x="470" y="40" width="120" height="30" fill="none" stroke="#000"/>
  <text x="530" y="60" font-size="14" text-anchor="middle">Depth 0</text>
  <line x1="470" y1="55" x2="350" y2="55" stroke="#f00" stroke-width="2"/>
  
  <rect x="470" y="140" width="120" height="30" fill="none" stroke="#000"/>
  <text x="530" y="160" font-size="14" text-anchor="middle">Depth 1</text>
  <line x1="470" y1="155" x2="400" y2="155" stroke="#f00" stroke-width="2"/>
  
  <rect x="470" y="250" width="120" height="30" fill="none" stroke="#000"/>
  <text x="530" y="270" font-size="14" text-anchor="middle">Depth 2</text>
  <line x1="470" y1="265" x2="450" y2="265" stroke="#f00" stroke-width="2"/>
  
  <!-- Height Labels -->
  <rect x="80" y="40" width="120" height="30" fill="none" stroke="#000"/>
  <text x="140" y="60" font-size="14" text-anchor="middle">Height 2</text>
  <line x1="140" y1="70" x2="300" y2="55" stroke="#00f" stroke-width="2"/>
  
  <rect x="80" y="140" width="120" height="30" fill="none" stroke="#000"/>
  <text x="140" y="160" font-size="14" text-anchor="middle">Height 1</text>
  <line x1="140" y1="160" x2="200" y2="160" stroke="#00f" stroke-width="2"/>
  
  <rect x="80" y="250" width="120" height="30" fill="none" stroke="#000"/>
  <text x="140" y="270" font-size="14" text-anchor="middle">Height 0</text>
  <line x1="140" y1="270" x2="350" y2="270" stroke="#00f" stroke-width="2"/>
</svg>

## Pruning
Pruning, in the context of machine learning and decision trees, is a technique used to reduce the size of trees by removing sections of the tree that do not provide power to classify instances. The goal of pruning is to improve the prediction accuracy of a decision tree model, or conversely to prevent overfitting.

When a subset of nodes (a sub-tree) doesn't significantly contribute to the prediction accuracy, it can be pruned (removed) to simplify the model.

<svg viewBox="0 0 800 300" xmlns="http://www.w3.org/2000/svg">
  <!-- Original Tree -->
  <rect x="130" y="20" width="100" height="60" fill="#ffcdd2" stroke="#000" rx="10" ry="10"/>
  <text x="180" y="55" font-size="16" text-anchor="middle">X ≤ 1</text>
  
  <line x1="180" y1="80" x2="100" y2="130" stroke="#000" stroke-width="2"/>
  <text x="130" y="115" font-size="12">T</text>
  
  <line x1="180" y1="80" x2="260" y2="130" stroke="#000" stroke-width="2"/>
  <text x="230" y="115" font-size="12">F</text>
  
  <rect x="60" y="130" width="80" height="60" fill="#c8e6c9" stroke="#000" rx="10" ry="10"/>
  <text x="100" y="165" font-size="16" text-anchor="middle">Y=1.5</text>
  
  <rect x="220" y="130" width="80" height="60" fill="#ffcdd2" stroke="#000" rx="10" ry="10"/>
  <text x="260" y="165" font-size="16" text-anchor="middle">X ≤ 2</text>
  
  <path d="M260,190 L220,240" stroke="#666" stroke-width="2" stroke-dasharray="5,5"/>
  <text x="230" y="225" font-size="12">T</text>
  
  <path d="M260,190 L300,240" stroke="#666" stroke-width="2" stroke-dasharray="5,5"/>
  <text x="290" y="225" font-size="12">F</text>
  
  <rect x="180" y="240" width="80" height="60" fill="#c8e6c9" stroke="#000" rx="10" ry="10"/>
  <text x="220" y="275" font-size="16" text-anchor="middle">Y=2.2</text>
  
  <rect x="260" y="240" width="80" height="60" fill="#c8e6c9" stroke="#000" rx="10" ry="10"/>
  <text x="300" y="275" font-size="16" text-anchor="middle">Y=1.5</text>
  
  <!-- Scissors Image -->
  <path d="M350,170 C360,160 370,170 360,180 L340,160 C350,150 360,160 350,170 Z" fill="#333" stroke="#000"/>
  <path d="M340,180 C330,170 320,180 330,190 L350,170 C340,160 330,170 340,180 Z" fill="#333" stroke="#000"/>
  <path d="M310,140 L360,190" stroke="#000" stroke-width="2"/>
  
  <!-- Pruned Tree -->
  <rect x="530" y="20" width="100" height="60" fill="#ffcdd2" stroke="#000" rx="10" ry="10"/>
  <text x="580" y="55" font-size="16" text-anchor="middle">X ≤ 1</text>
  
  <line x1="580" y1="80" x2="500" y2="130" stroke="#000" stroke-width="2"/>
  <text x="530" y="115" font-size="12">T</text>
  
  <line x1="580" y1="80" x2="660" y2="130" stroke="#000" stroke-width="2"/>
  <text x="630" y="115" font-size="12">F</text>
  
  <rect x="460" y="130" width="80" height="60" fill="#c8e6c9" stroke="#000" rx="10" ry="10"/>
  <text x="500" y="165" font-size="16" text-anchor="middle">Y=1.5</text>
  
  <rect x="620" y="130" width="80" height="60" fill="#b3e5fc" stroke="#000" rx="10" ry="10"/>
  <text x="660" y="165" font-size="16" text-anchor="middle">Y=1.9</text>
  
  <!-- Arrow -->
  <path d="M380,150 L460,150" stroke="#060" stroke-width="4" marker-end="url(#arrowhead)"/>
  <defs>
    <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="0" refY="3.5" orient="auto">
      <polygon points="0 0, 10 3.5, 0 7" fill="#060"/>
    </marker>
  </defs>
  
  <!-- Description Box -->
  <rect x="180" y="40" width="400" height="70" fill="white" stroke="#000" fill-opacity="0.8" rx="10" ry="10"/>
  <text x="380" y="65" font-size="12" text-anchor="middle">The subtree of x≤2 didn't contribute to the prediction accuracy so we</text>
  <text x="380" y="85" font-size="12" text-anchor="middle">prune it and replaced it with a leaf with the average value of the original</text>
  <text x="380" y="105" font-size="12" text-anchor="middle">subtree leaves. (Y=1.9 is the average of Y=2.2 and Y=1.5)</text>
</svg>

## Gini Impurity
Until now we saw common terms in tree data structure but we still need to understand how we can convert our dataset into a decision tree.

In machine learning, when building decision trees, we need a metric or measurement to help the algorithm decide how to split the data at each node.

The most common information measurement that will help our algorithm decide how to build the decision tree from the provided data set is called Gini Impurity.

Gini Impurity → Gini Impurity is a metric used in decision trees algorithm for the task of decision making like deciding where to split the data. It measures the degree or probability of a specific variable being wrongly classified when it is randomly picked.

### Gini Impurity Formula
Gini Impurity gives a measure of how often a randomly chosen element from a set would be incorrectly labeled if it was randomly labeled according to the distribution of labels in the subset. The algorithm will try to minimize the Gini Impurity to make the best splits.

Gini Impurity formula for classification problems is the following:
- Q → Specific node in the decision tree
- G(Q) → The Gini Impurity value of that Q node
- Pc → The probability of belonging to a class C in Q node
- 1 - Pc → The probability of not belonging to a class C in Q node
- Σ → Sum of the probabilities of all classes in Q node

G(Q) = Σ pc(1 - pc)

### Example: Binary Classification
For binary classification problems:
- If Pc = 0.5 → G(Q) = 0.5 (maximum impurity)
- If Pc = 0 or 1 → G(Q) = 0 (minimum impurity/pure node)

When we have the most "pure" dataset (all data points belong to one class), we get a Gini Impurity value of 0.
When we have the most "impure" dataset (equal distribution between classes), we get a Gini Impurity value of 0.5.

<svg viewBox="0 0 800 500" xmlns="http://www.w3.org/2000/svg">
  <!-- Pure Node Example -->
  <rect x="50" y="50" width="220" height="150" fill="none" stroke="#333" rx="10" ry="10"/>
  <circle cx="100" cy="100" r="20" fill="#a5d6a7" stroke="#000"/>
  <circle cx="150" cy="100" r="20" fill="#a5d6a7" stroke="#000"/>
  <circle cx="100" cy="150" r="20" fill="#a5d6a7" stroke="#000"/>
  <circle cx="150" cy="150" r="20" fill="#a5d6a7" stroke="#000"/>
  
  <path d="M180,125 L220,125" stroke="#000" stroke-width="2" stroke-dasharray="5,5"/>
  <path d="M220,125 L270,100" stroke="#000" stroke-width="2" stroke-dasharray="5,5"/>
  <path d="M220,125 L270,150" stroke="#000" stroke-width="2" stroke-dasharray="5,5"/>
  
  <!-- Class Distribution Boxes -->
  <rect x="280" y="80" width="200" height="40" fill="#ffcdd2" stroke="#000"/>
  <text x="380" y="105" font-size="14" text-anchor="middle">Class Red (0/4)(1-0/4) = 0</text>
  
  <text x="380" y="135" font-size="16" text-anchor="middle">+</text>
  
  <rect x="280" y="150" width="200" height="40" fill="#bbdefb" stroke="#000"/>
  <text x="380" y="175" font-size="14" text-anchor="middle">Class Blue (4/4)(1-4/4) = 0</text>
  
  <!-- Gini Result -->
  <rect x="500" y="115" width="200" height="40" fill="#ffe0b2" stroke="#000"/>
  <text x="600" y="140" font-size="14" text-anchor="middle">Gini Impurity = 0 + 0 = 0</text>
  
  <path d="M480,135 L500,135" stroke="#060" stroke-width="2" marker-end="url(#arrowhead2)"/>
  
  <!-- Impure Node Example -->
  <rect x="50" y="250" width="220" height="150" fill="none" stroke="#333" rx="10" ry="10"/>
  <circle cx="100" cy="300" r="20" fill="#ef9a9a" stroke="#000"/>
  <circle cx="150" cy="300" r="20" fill="#bbdefb" stroke="#000"/>
  <circle cx="100" cy="350" r="20" fill="#ef9a9a" stroke="#000"/>
  <circle cx="150" cy="350" r="20" fill="#bbdefb" stroke="#000"/>
  
  <path d="M180,325 L220,325" stroke="#000" stroke-width="2" stroke-dasharray="5,5"/>
  <path d="M220,325 L270,300" stroke="#000" stroke-width="2" stroke-dasharray="5,5"/>
  <path d="M220,325 L270,350" stroke="#000" stroke-width="2" stroke-dasharray="5,5"/>
  
  <!-- Class Distribution Boxes -->
  <rect x="280" y="280" width="200" height="40" fill="#ffcdd2" stroke="#000"/>
  <text x="380" y="305" font-size="14" text-anchor="middle">Class Red (2/4)(1-2/4) = 0.25</text>
  
  <text x="380" y="335" font-size="16" text-anchor="middle">+</text>
  
  <rect x="280" y="350" width="200" height="40" fill="#bbdefb" stroke="#000"/>
  <text x="380" y="375" font-size="14" text-anchor="middle">Class Blue (2/4)(1-2/4) = 0.25</text>
  
  <!-- Gini Result -->
  <rect x="500" y="315" width="200" height="40" fill="#ffe0b2" stroke="#000"/>
  <text x="600" y="340" font-size="14" text-anchor="middle">Gini Impurity = 0.25 + 0.25 = 0.5</text>
  
  <path d="M480,335 L500,335" stroke="#060" stroke-width="2" marker-end="url(#arrowhead2)"/>
  
  <!-- Gini Impurity Graph -->
  <rect x="170" y="430" width="460" height="40" fill="none" stroke="#000"/>
  <text x="400" y="455" font-size="14" text-anchor="middle">Gini Impurity Value Range for Binary Classification (0 to 0.5)</text>
  <line x1="200" y1="420" x2="600" y2="420" stroke="#000" stroke-width="2"/>
  <line x1="200" y1="420" x2="200" y2="415" stroke="#000" stroke-width="2"/>
  <line x1="600" y1="420" x2="600" y2="415" stroke="#000" stroke-width="2"/>
  <text x="200" y="410" font-size="12" text-anchor="middle">0</text>
  <text x="600" y="410" font-size="12" text-anchor="middle">0.5</text>
  <text x="150" y="420" font-size="12" text-anchor="end">Pure</text>
  <text x="650"

### Gini Impurity Example - Email Spam Detection
Let's construct a simple decision tree from a dataset about emails with URL links and whether they are spam:

- Feature (X): Email contains URL link (Yes/No)
- Label (Y): Email is spam (Yes/No)

For emails containing URL links:
- 2 emails were spam
- 1 email was not spam
- Gini impurity = (2/3)(1-2/3) + (1/3)(1-1/3) = 0.44

For emails not containing URL links:
- 1 email was spam
- 3 emails were not spam
- Gini impurity = (1/4)(1-1/4) + (3/4)(1-3/4) = 0.375

Total tree Gini impurity = (3/7)(0.44) + (4/7)(0.375) = 0.403

## Class Exercise - Car Classification
Instructions:
You are working as a data scientist in a car company and provided with a dataset of 10 cars with their color (Red or Blue) and whether they are sports cars. Your mission is to predict if a new car will be a sports car.

1. Construct a simple decision tree based on the given dataset
2. Calculate for each leaf node the Gini Impurity value
3. Calculate the entire tree Gini Impurity value