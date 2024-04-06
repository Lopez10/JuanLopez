---
title: "Entities and value objects"
description: ".fdsafdsafdsafdsafasdfsadfadsfdsafdsafdsafasdfasdfdsfdsafdasfdsafdasfdsa"
publishDate: "28 March 2024"
tags: ["DDD", "Value object", "Entity"]
draft: true
---

¿Alguna vez te has preguntado por qué diferenciar "Value objects" y "Entidades" es crucial en el diseño de software?

Para entender la diferencia, imagina a Felipe López, un hombre de 44 años. Su nombre, dni y edad por separado son Value Objects (inmutables y sin identidad propia). Pero juntando todos los value objects, crean una Entidad usuario (única e identificable).

Vamos a ver un ejemplo de como crear el value object dni. 

```ts title="Restaurant.entity.ts"
class Dni {
    private constructor(private value: string) {
        this.value = value;
    }

    
    static create(value: string): Dni {
        if (value.length !== 9) throw new Error('DNI must have 9 characteres');

        const numbers = value.substring(0,7)

        const letter = value.substring(7)
                    
        return new Dni(value);
    }
}
```