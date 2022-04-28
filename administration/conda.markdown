# Conda, Anaconda, Miniconda...

J'utilise conda avant tout pour gérer des notebooks Jupyter.

## FAQ

- Différence entre Jupyter notebooks et Jupyterlab:

TODO

- Différence entre Conda, Anaconda et Miniconda:
  - Anaconda, c'est toute une distribution qui inclue conda, jupyter, anaconda-navigator...
  - [miniconda](https://docs.conda.io/en/latest/miniconda.html), c'est la même chose mais avec le minimum de packages installés par défaut.
  - conda est le package manager et gestionnaire d'environnment inclus.

## Installer Anaconda

```bash
sh Anaconda3-2021.11-Linux-x86_64.sh
# Le script demande si on veut lancer conda init, dans tous les cas il ne recouvre
# pas ce qui est déjà dans .bashrc.

# désactive l'environnement base par défaut, je fais ça pour éviter de tout
# mélanger.
conda config --set auto_activate_base false
```

## Désinstaller Anaconda

J'installe Anaconda dans le répertoire par défault: `$HOME/anaconda3`.

Lancer `conda init` ajoute ceci dans mon bashrc.

```bash
# $HOME/.bashrc
# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('$HOME/anaconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "$HOME/anaconda3/etc/profile.d/conda.sh" ]; then
        . "$HOME/anaconda3/etc/profile.d/conda.sh"
    else
        export PATH="$HOME/anaconda3/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<
```

Pas besoin de supprimer ce code si je supprime le répertoire d'installation vu
que les cas d'erreur sont bien gérés. (Si conda n'existe pas, ça passe à la suite
sans faire planter le terminal.)

## pkgs/main vs conda-forge

Quand on mélange des installation depuis [conda-forge](https://anaconda.org/conda-forge/repo) avec la chaîne
par défaut [https://repo.anaconda.com/pkgs/main/linux-64/](pkgs/main), on obtient souvent ce genre de messages:

```
The following packages will be SUPERSEDED by a higher-priority channel:

  certifi            conda-forge::certifi-2021.10.8-py39hf~ --> pkgs/main::certifi-2021.10.8-py39h06a4308_2
```

Ce changement est limité à l'environnement dans lequel on se trouve, en ce qui concerne les
environnements des notebooks Jupyter, ça n'est sûrement pas trop grave.

## Les environnements

Pour l'instant, je pense qu'à partir du moment où un projet a des dépendances
il doit avoir sont propre environnement.

Tous les binaires de l'environnement `base` sont dans `$HOME/anaconda3/bin`.
Pour tous les environnements créés, c'est dans `$HOME/anaconda3/envs/$ENV_NAME/bin`.

## Les changements de PATH (TODO)

Quand est-ce qu'on a accès à quoi?

- Quand `conda config --set auto_activate_base false`
- Quand `conda config --set auto_activate_base true`
- Quand `conda deactivate`
- Quand `conda activate`

## Dans quel environnement conda est-il installé?

L'éxecutable `conda` est dans `anaconda3/condabin` et est accessible dans le path.
Par contre, qu'en est-il une fois qu'on a créé un environnement? On a une version
de conda à l'intérieur?

## Installer Geopandas

Quel enfer.
