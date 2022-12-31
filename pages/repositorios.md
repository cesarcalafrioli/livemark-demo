# Repositorios

<div class="livemarks">
    {% for row in frictionless.extract('data/repositorios.csv') %}
    <div class="item">
        <div class="item-content">
            <h3>
                <a href="https://github.com/{{ row.user }}/{{ row.repo }}" target="_blank" style="color: black"> {{ row.repo }}
                </a>
            </h3>
            <p>{{ row.description or 'Descricao nao disponível' }}</p>
            <a href="https://img.shields.io/github/license/{{ row.user }}/{{ row.repo }}"><img src="https://img.shields.io/github/license/{{ row.user }}/{{ row.repo }}" /></a>
            <a href="https://img.shields.io/github/issues/{{ row.user }}/{{ row.repo }}"><img src="https://img.shields.io/github/issues/{{ row.user }}/{{ row.repo }}" /></a>
            <a href="https://img.shields.io/github/forks/{{ row.user }}/{{ row.repo }}"><img src="https://img.shields.io/github/forks/{{ row.user }}/{{ row.repo }}" /></a>
            <a href="https://img.shields.io/github/stars/{{ row.user }}/{{ row.repo }}"><img src="https://img.shields.io/github/stars/{{ row.user }}/{{ row.repo }}" /></a>
            <a href="https://img.shields.io/github/stars/{{ row.user }}/{{ row.repo }}"><img src="https://img.shields.io/github/stars/{{ row.user }}/{{ row.repo }}" /></a>                    
            <p>
                <a class="item-content-link" href="https://github.com/{{ row.user }}/{{ row.repo }}" target="_blank"> Github <span class="fa fa-external-link-alt"></span>
                </a>
            </p>
        </div>
    </div>
    {% endfor %}
</div>