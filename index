import { Principal } from 'azle';
import { ic } from 'azle';

type Character = {
    id: string;
    owner: Principal;
    type: string; // e.g., animal, superhero, car
    name: string;
};

type Trade = {
    id: string;
    seller: Principal;
    characterId: string;
    price: bigint; // ICP is represented in e8s (1 ICP = 100_000_000 e8s)
};

let characters: Character[] = [];
let trades: Trade[] = [];

// Create a new virtual character
export function createCharacter(type: string, name: string): Character {
    const character: Character = {
        id: `${characters.length + 1}`,
        owner: ic.caller(),
        type,
        name,
    };
    characters.push(character);
    return character;
}

// List a character for sale
export function listCharacterForSale(characterId: string, price: bigint): Trade | null {
    const characterIndex = characters.findIndex((char) => char.id === characterId && char.owner === ic.caller());
    if (characterIndex === -1) {
        return null;
    }
    const trade: Trade = {
        id: `${trades.length + 1}`,
        seller: ic.caller(),
        characterId,
        price,
    };
    trades.push(trade);
    return trade;
}

// Purchase a character
export function purchaseCharacter(tradeId: string): Character | null {
    const tradeIndex = trades.findIndex((trade) => trade.id === tradeId);
    if (tradeIndex === -1) {
        return null;
    }
    const trade = trades[tradeIndex];
    
    // Note: Simplified for demonstration. Real scenario should handle ICP transfer.
    const characterIndex = characters.findIndex((char) => char.id === trade.characterId);
    if (characterIndex !== -1) {
        const character = characters[characterIndex];
        character.owner = ic.caller(); // Transfer ownership
        trades.splice(tradeIndex, 1); // Remove the trade
        return character;
    }
    return null;
}

// Retrieve all available characters for sale
export function getCharactersForSale(): Trade[] {
    return trades;
}

// Retrieve characters owned by the caller
export function getMyCharacters(): Character[] {
    return characters.filter((char) => char.owner === ic.caller());
}
