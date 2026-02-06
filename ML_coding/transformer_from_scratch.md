# Transformer from Scratch

## Problem Description
Implement a **Transformer** architecture from scratch using PyTorch. The implementation should be concise, effective, and follow the logical structure of modern Transformer models (e.g., self-attention, multi-head attention, and transformer blocks).

Reference: [Transformers from Scratch by Peter Bloem](https://peterbloem.nl/blog/transformers)

## Approach
A Transformer consists of several modular components:
1.  **Self-Attention**: The core mechanism where each token in a sequence attends to all other tokens.
2.  **Multi-Head Attention**: Running multiple self-attention operations in parallel to capture different types of relationships.
3.  **Transformer Block**: A layer consisting of Multi-Head Attention, followed by Layer Normalization, a Feed-Forward network, and Residual Connections.
4.  **The Transformer**: A stack of these blocks, preceded by an Embedding layer and a Position Encoding layer.

## Code Implementation (PyTorch)

```python
import torch
from torch import nn
import torch.nn.functional as F

class SelfAttention(nn.Module):
    def __init__(self, k, heads=8):
        super().__init__()
        self.k, self.heads = k, heads
        # We compute all heads in one go using a single linear layer
        self.tokeys = nn.Linear(k, k * heads, bias=False)
        self.toqueries = nn.Linear(k, k * heads, bias=False)
        self.tovalues = nn.Linear(k, k * heads, bias=False)
        self.unifyheads = nn.Linear(k * heads, k)

    def forward(self, x):
        b, t, k = x.size()
        h = self.heads

        queries = self.toqueries(x).view(b, t, h, k)
        keys = self.tokeys(x).view(b, t, h, k)
        values = self.tovalues(x).view(b, t, h, k)

        # Fold heads into the batch dimension
        queries = queries.transpose(1, 2).reshape(b * h, t, k)
        keys = keys.transpose(1, 2).reshape(b * h, t, k)
        values = values.transpose(1, 2).reshape(b * h, t, k)

        # Scaled Dot-Product Attention
        dot = torch.bmm(queries, keys.transpose(1, 2)) / (k ** 0.5)
        dot = F.softmax(dot, dim=2)

        out = torch.bmm(dot, values).view(b, h, t, k)
        out = out.transpose(1, 2).reshape(b, t, h * k)
        return self.unifyheads(out)

class TransformerBlock(nn.Module):
    def __init__(self, k, heads):
        super().__init__()
        self.attention = SelfAttention(k, heads=heads)
        self.norm1 = nn.LayerNorm(k)
        self.norm2 = nn.LayerNorm(k)
        self.ff = nn.Sequential(
            nn.Linear(k, 4 * k),
            nn.ReLU(),
            nn.Linear(4 * k, k)
        )

    def forward(self, x):
        # Attention + Residual + Norm
        attended = self.attention(x)
        x = self.norm1(attended + x)
        # Feed-Forward + Residual + Norm
        fedforward = self.ff(x)
        return self.norm2(fedforward + x)

class Transformer(nn.Module):
    def __init__(self, k, heads, depth, seq_length, num_tokens, num_classes):
        super().__init__()
        self.num_tokens = num_tokens
        self.token_emb = nn.Embedding(num_tokens, k)
        self.pos_emb = nn.Embedding(seq_length, k)

        # The core Transformer stack
        blocks = [TransformerBlock(k, heads) for _ in range(depth)]
        self.blocks = nn.Sequential(*blocks)

        # Output head
        self.toprobs = nn.Linear(k, num_classes)

    def forward(self, x):
        b, t = x.size()
        tokens = self.token_emb(x)
        positions = torch.arange(t, device=x.device)
        positions = self.pos_emb(positions)[None, :, :].expand(b, t, k)
        
        x = tokens + positions
        x = self.blocks(x)
        
        # Average over sequence dimension (global average pooling)
        x = x.mean(dim=1)
        return self.toprobs(x)

# --- Test Case ---
if __name__ == "__main__":
    # Hyperparameters
    K = 128
    HEADS = 8
    DEPTH = 6
    SEQ_LENGTH = 100
    NUM_TOKENS = 1000
    NUM_CLASSES = 10

    model = Transformer(K, HEADS, DEPTH, SEQ_LENGTH, NUM_TOKENS, NUM_CLASSES)
    
    # Dummy input (batch_size=4, seq_len=100)
    dummy_input = torch.randint(0, NUM_TOKENS, (4, SEQ_LENGTH))
    output = model(dummy_input)
    
    print(f"Input Shape: {dummy_input.shape}")
    print(f"Output Shape: {output.shape}")
    assert output.shape == (4, NUM_CLASSES)
    print("Transformer implementation successful!")
```

## Complexity Analysis
-   **Time Complexity**: $O(T^2 \cdot K)$ per layer, where $T$ is the sequence length and $K$ is the embedding dimension. The quadratic term $T^2$ comes from the self-attention matrix.
-   **Space Complexity**: $O(T^2)$ per head per layer to store the attention weights during the forward pass.

