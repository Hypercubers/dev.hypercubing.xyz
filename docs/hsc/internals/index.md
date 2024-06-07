---
title: Hyperspeedcube Internals
---

# Hyperspeedcube Internals

The goal of this tutorial series is to explain the entirety of the Hyperspeedcube puzzle engine, including construction and simulation. It proritizes geometric intuition for the mathematics and algorithms involved, but doesn't provide formal proofs. Especially in the earlier chapters, you'll just have to believe me that the math works the way it does.

This series is meant to be read in order; if you don't understand something, try rereading earlier pages until they make sense. If it still doesn't make sense after rereading, ping me (**@HactarCE** or **@Hyperspeedcube Developer**) on the Hypercubers Discord server.

There are occasional exercises throughout. Even for a casual read, I highly recommend completing these exercises; they're designed to take very little time and will massively improve your understanding of the material.

Read [Prequisites](../prereqs.md) first to make sure you have the prerequisite knowledge.

## Code structure

Hyperspeedcube is split into four crates, each depending on the previous ones:

- **`hypermath`** - mathematical primitives (vectors, matrixes, CGA multivectors, and some data structures)
- **`hypershape`** - shape slicing algorithms
- **`hyperpuzzle`** - Lua API and any algorithms that are relevant particularly to twisty puzzles
- **`hyperspeedcube`** - user interface

This tutorial series describes how the first three crates work. The `hyperspeedcube` crate contains typical UI code and doesn't particularly need external documentation.

## Roadmap

Hyperspeedcube makes heavy use of an algebraic system called **Conformal Geometric Algebra** (**CGA**). In order to understand CGA, you must first understand **Projective Geometric Algebra** (**PGA**), which in turn relies on an understanding of **Vectorspace Geometric Algebra** (**VGA**).

Once we have established a solid understanding of CGA, we'll cover the representation of conformal polytopes and various algorithms we can do using them, culminating in the shape slicing algorithm.

After that, we'll define patchwork spaces and extend the shape slicing algorithm to work in those.
