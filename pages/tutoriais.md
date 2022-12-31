# Tutoriais

Meus tutoriais armazenados no github.

<div class="livemarks">
    {% for row in frictionless.extract('data/tutoriais.csv') %}
    <div class="item">
        <div class="item-content">
            <h3>
                <a href="https://github.com/{{ row.user }}/{{ row.repo }}" target="_blank" style="color: black"> {{ row.repo }}
                </a>
            </h3>
            <p>{{ row.description or 'Descricao nao disponível' }}</p>
            <p>
                <a class="item-content-link" href="https://github.com/{{ row.user }}/{{ row.repo }}" target="_blank"> Github <span class="fa fa-external-link-alt"></span>
                </a>
            </p>
        </div>
        <div class="item_stars">
            <span class="fa-stack fa-2x">
                <i class="fas fa-stack-2x fa-star fa-inverse item-stars-icon"></i>
                <i class="fas fa-stack-1 item-stars-count">{{ row.stars }}</i>
            </span>
        </div>
    </div>
    {% endfor %}
</div>