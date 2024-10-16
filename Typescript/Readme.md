# Typescript

## Why use `satisfies` keyowrd?
You want to ensure an object satisfies a type without creating a new type.

## How Typescript do type check for an object?
TypeScript structurally checks if this object satisfies at least one of the union types, and then check other entry if they belong to any of the union. 

TypeScript performs excess property checks when assigning object literals to union types. This means that if you try to add properties that are not defined in either of the union types, TypeScript will flag this as an error.
