# genpass

This `genpass` script generates passphrases that have the advantage of being
somewhat pronounceable and memorizable while having a known entropy.

Here's a sample execution:

```
$ genpass
lyvi hesy hepu wumo voju biwi teby gyhe lehy tacu gozo joqa reny wymy tafo
  14   28   41   55   69   83   97  111  124  138  152  166  180  193  207 bits
```

The number of bits shown for each length is the number of bits of
entropy. This is what matters for password strength.

## What is entropy?

Entropy, in [information
theory](https://en.wikipedia.org/wiki/Information_theory),
is a numeric value expressed in _bits of information_ for a
_probability distribution_. It means that if a system requiring a
secret key is implemented and deployed properly, it would take close
to 2<sup>N</sup> attempts to guess a key picked from a
distribution of entropy N.

If you want to understand entropy, it is important to realize that
it's not a property of a passphrase. When we say "this passphrase
has 80 bits of entropy", we actually mean "this passphrase was picked
at random according to a distribution whose entropy is 80 bits".

For example, it is possible that our generator produces
`cafe cafe cafe cafe cafe cafe` with the same minuscule probability as
`lyvi hesy hepu wumo voju biwi`. Would it be easy to guess
in practice? Yes. Is the entropy any different? No.

## What is a good passphrase length?

tl;dr 80+ bits of entropy as of 2026

Actual password security depends on a bunch of other factors.
I recommend reading
https://it.physics.gla.ac.uk/notes/security/passwords.

## How do you calculate entropy for this password generator?

Excellent question. Glad you asked.

A passphrase is generated as a sequence of "syllables" made of a
consonant and a vowel. There are 120 syllables to pick from because
they're formed from one of 20 consonants (excluding `y`) and one of
6 vowels, giving us 20 × 6 = 120 combinations. Successive syllables are
chosen independently i.e. without trying to form nice-sounding words.
This gives us an entropy of log<inf>2</inf>(120) bits per syllable which
is approximately 6.906 bits. Multiply it by the number of syllables
to generate and you get the entropy.
Generating 12 syllables as in `lyvi hesy hepu wumo voju biwi`
corresponds to 12 × log<inf>2</inf>(120) ≃ 82.88 bits of entropy.

Keep in mind that entropy is not in the passphrase but in the process
that generates the passphrase. So, if you generate two passphrases and
pick the one you prefer, you lose entropy! More precisely, you lose up
to log<inf>2</inf>(2) = 1 bit. The extreme cases are:

- If you pick among the two passphrases randomly or if you pick
  systematically the first one, you don't lose any entropy.
- If your choice is perfectly predictable to an attacker, you lose the
  full bit.

If you pick the passphrase that for some reason you like better,
in general you would lose more than 0 but less than 1 bit.

Likewise, if you generate 8 passphrases and you pick the one you like
best, you lose up to log<inf>2</inf>(8) = 3 × log<inf>2</inf>(2) = 3
bits.

However, if you don't use the password generator at all and rely on
your brain to generate a passphrase, you make the passphrase much
easier to guess even it has the right consonant/vowel structure.
Using your brain is a different process than using the password
generator and therefore has a different entropy, in fact
a much lower one. Don't trust your brain to produce random
symbols, it's pretty bad at it.

## Is this `genpass` password generator secure?

It uses Python's `secrets` library which was designed for this
purpose. The `genpass` script is sufficiently short that experts can
review it easily.

In general, when it comes to software security tooling
and you don't know what you're doing, it's best to stick
with tools backed by trustworthy and responsible organizations.
