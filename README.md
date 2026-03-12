import { Link } from 'react-router-dom';
import { 
  Truck, 
  Shield, 
  CreditCard, 
  Headphones, 
  ArrowRight, 
  MessageCircle,
  Home,
  Search,
  ShoppingCart,
  User,
  Star,
  Clock,
  Award,
  Phone
} from 'lucide-react';
import { Helmet } from 'react-helmet';
import { useEffect, useState, memo } from 'react';
import { categories, products } from '@/data/products';
import { VehicleSelector } from '@/components/store/VehicleSelector';
import { ProductCard } from '@/components/store/ProductCard';
import { Button } from '@/components/ui/button';

// ============================================
// CONSTANTES E CONFIGURAÇÕES
// ============================================

const benefits = [
  { icon: Shield, title: 'Garantia de Compatibilidade', desc: 'Peças verificadas para seu veículo' },
  { icon: Truck, title: 'Entrega Rápida', desc: 'Envio para todo o Brasil' },
  { icon: CreditCard, title: 'Parcele em 12x', desc: 'Sem juros no cartão' },
  { icon: Headphones, title: 'Suporte Técnico', desc: 'Equipe especializada' },
];

const quickLinks = [
  { icon: Home, label: 'Home', path: '/', active: true },
  { icon: Search, label: 'Buscar', path: '/catalogo', active: false },
  { icon: ShoppingCart, label: 'Carrinho', path: '/carrinho', active: false, badge: 3 },
  { icon: User, label: 'Conta', path: '/conta', active: false },
];

const WHY_CHOOSE_US = [
  { icon: Award, title: 'Qualidade OEM', desc: 'Peças originais e certificadas' },
  { icon: Clock, title: 'Pronta Entrega', desc: 'Estoque atualizado diariamente' },
  { icon: Phone, title: 'Atendimento', desc: 'Suporte técnico especializado' },
  { icon: Star, title: 'Confiança', desc: 'Mais de 10.000 clientes atendidos' },
];

// ============================================
// COMPONENTES OTIMIZADOS
// ============================================

// Memoized Product Card para performance
const ProductCardMemo = memo(({ product }) => (
  <ProductCard product={product} />
));

ProductCardMemo.displayName = 'ProductCardMemo';

// Componente do Botão WhatsApp
const WhatsAppButton = () => (
  <div className="fixed bottom-6 right-6 z-50">
    <a
      href="https://wa.me/5511999999999?text=Olá!%20Vim%20da%20Droop%20Race%20e%20preciso%20de%20ajuda%20com%20peças%20para%20meu%20veículo"
      target="_blank"
      rel="noopener noreferrer"
      className="flex items-center gap-2 bg-green-500 text-white px-4 py-3 rounded-full shadow-lg hover:bg-green-600 transition-all hover:scale-105 hover:shadow-xl group"
      aria-label="Fale conosco no WhatsApp"
    >
      <MessageCircle className="h-5 w-5 group-hover:animate-pulse" />
      <span className="hidden sm:inline font-medium">Precisa de ajuda?</span>
    </a>
  </div>
);

// Componente de Newsletter
const NewsletterSection = () => {
  const [email, setEmail] = useState('');
  const [isSubmitting, setIsSubmitting] = useState(false);

  const handleSubmit = async (e) => {
    e.preventDefault();
    setIsSubmitting(true);
    // Simular envio
    await new Promise(resolve => setTimeout(resolve, 1000));
    alert('Obrigado por se cadastrar! Você receberá 10% OFF no seu primeiro e-mail.');
    setEmail('');
    setIsSubmitting(false);
  };

  return (
    <section className="bg-gradient-to-r from-primary/10 via-primary/5 to-transparent border-t border-border">
      <div className="container mx-auto px-4 py-16">
        <div className="max-w-2xl mx-auto text-center">
          <div className="inline-flex items-center justify-center w-16 h-16 rounded-full bg-primary/20 mb-6">
            <Award className="h-8 w-8 text-primary" />
          </div>
          <h3 className="font-display text-3xl md:text-4xl font-bold mb-3">
            Receba Ofertas Exclusivas
          </h3>
          <p className="text-muted-foreground text-lg mb-8">
            Cadastre-se e ganhe <span className="text-primary font-bold">10% OFF</span> na primeira compra + 
            <span className="font-medium"> novidades e dicas de manutenção</span>
          </p>
          <form onSubmit={handleSubmit} className="flex flex-col sm:flex-row gap-3 max-w-md mx-auto">
            <input
              type="email"
              value={email}
              onChange={(e) => setEmail(e.target.value)}
              placeholder="Seu melhor e-mail"
              required
              className="flex-1 px-6 py-4 rounded-xl border-2 border-border bg-background focus:outline-none focus:ring-2 focus:ring-primary focus:border-transparent transition-all"
            />
            <Button 
              type="submit" 
              disabled={isSubmitting}
              className="whitespace-nowrap px-8 py-4 text-base font-semibold"
            >
              {isSubmitting ? 'Enviando...' : 'Quero 10% OFF'}
            </Button>
          </form>
          <p className="text-xs text-muted-foreground mt-4">
            Ao se cadastrar, você concorda com nossa Política de Privacidade
          </p>
        </div>
      </div>
    </section>
  );
};

// Bottom Navigation para mobile
const BottomNav = () => (
  <div className="fixed bottom-0 left-0 right-0 bg-card border-t border-border md:hidden z-40">
    <div className="flex justify-around items-center h-16">
      {quickLinks.map((item) => (
        <Link
          key={item.label}
          to={item.path}
          className={`flex flex-col items-center justify-center flex-1 h-full transition-colors ${
            item.active 
              ? 'text-primary' 
              : 'text-muted-foreground hover:text-primary'
          }`}
        >
          <item.icon className={`h-5 w-5 ${item.badge ? 'relative' : ''}`} />
          {item.badge && (
            <span className="absolute -top-1 -right-1 bg-accent text-white text-xs rounded-full h-4 w-4 flex items-center justify-center">
              {item.badge}
            </span>
          )}
          <span className="text-xs mt-1">{item.label}</span>
        </Link>
      ))}
    </div>
  </div>
);

// ============================================
// COMPONENTE PRINCIPAL
// ============================================

export default function Index() {
  const featuredProducts = products.filter(p => p.inStock).slice(0, 4);
  const saleProducts = products.filter(p => p.originalPrice);
  const newProducts = products.filter(p => p.isNew).slice(0, 4);

  // Analytics tracking
  useEffect(() => {
    // Track page view
    if (typeof window.gtag !== 'undefined') {
      window.gtag('config', 'GA_MEASUREMENT_ID', {
        page_title: 'Droop Race Multimarcas - Home',
        page_path: '/',
      });
    }

    // Performance tracking
    if (window.performance) {
      const perfData = window.performance.timing;
      const pageLoadTime = perfData.loadEventEnd - perfData.navigationStart;
      console.log(`Página carregada em ${pageLoadTime}ms`);
    }
  }, []);

  return (
    <>
      {/* SEO */}
      <Helmet>
        <title>Droop Race Multimarcas | Peças Automotivas com Compatibilidade Garantida</title>
        <meta 
          name="description" 
          content="Encontre peças automotivas com compatibilidade garantida para seu veículo. Freios, suspensão, filtros e muito mais. Entrega rápida em todo Brasil." 
        />
        <meta 
          name="keywords" 
          content="peças automotivas, auto peças, freios, suspensão, filtros, iluminação, motor, multimarcas" 
        />
        <meta property="og:title" content="Droop Race Multimarcas | Peças Automotivas" />
        <meta property="og:description" content="Peças automotivas com compatibilidade garantida para seu veículo" />
        <meta property="og:type" content="website" />
        <meta property="og:url" content="https://drooprace.com.br" />
        <link rel="canonical" href="https://drooprace.com.br" />
      </Helmet>

      {/* Hero Section com imagem de fundo */}
      <section className="relative bg-gradient-to-br from-primary via-primary to-secondary overflow-hidden">
        {/* Overlay com imagem de fundo */}
        <div className="absolute inset-0 bg-black/40 z-10" />
        <div className="absolute inset-0 opacity-20">
          <img 
            src="/images/hero-bg.jpg" 
            alt=""
            className="w-full h-full object-cover"
            loading="eager"
            onError={(e) => {
              e.target.style.display = 'none'; // Fallback se imagem não existir
            }}
          />
        </div>
        
        {/* Conteúdo */}
        <div className="container mx-auto px-4 py-16 md:py-24 relative z-20">
          <div className="max-w-3xl mx-auto text-center">
            {/* Badge de confiança */}
            <div className="inline-flex items-center gap-2 bg-white/10 backdrop-blur-sm rounded-full px-4 py-2 mb-6">
              <Shield className="h-4 w-4 text-accent" />
              <span className="text-sm font-medium text-white">Loja Oficial • Mais de 10.000 peças</span>
            </div>

            <h1 className="font-display text-4xl md:text-6xl font-bold text-white mb-4 leading-tight">
              Droop Race <span className="text-accent">Multimarcas</span>
            </h1>
            
            <p className="text-white/90 text-lg md:text-xl mb-8 max-w-2xl mx-auto">
              Encontre a peça certa para seu veículo com garantia de compatibilidade. 
              Entrega rápida e suporte técnico especializado.
            </p>

            {/* Buscador */}
            <div className="max-w-2xl mx-auto bg-white/10 backdrop-blur-md rounded-2xl p-6">
              <VehicleSelector />
            </div>

            {/* Stats */}
            <div className="grid grid-cols-2 md:grid-cols-3 gap-6 mt-12">
              <div className="text-center">
                <div className="text-2xl font-bold text-white">10k+</div>
                <div className="text-sm text-white/70">Clientes Atendidos</div>
              </div>
              <div className="text-center">
                <div className="text-2xl font-bold text-white">15k+</div>
                <div className="text-sm text-white/70">Peças em Estoque</div>
              </div>
              <div className="text-center col-span-2 md:col-span-1">
                <div className="text-2xl font-bold text-white">24h</div>
                <div className="text-sm text-white/70">Entrega Rápida</div>
              </div>
            </div>
          </div>
        </div>

        {/* Onda decorativa */}
        <div className="absolute bottom-0 left-0 right-0">
          <svg viewBox="0 0 1440 120" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M0 120L60 110C120 100 240 80 360 70C480 60 600 60 720 65C840 70 960 80 1080 85C1200 90 1320 90 1380 90L1440 90V120H1380C1320 120 1200 120 1080 120C960 120 840 120 720 120C600 120 480 120 360 120C240 120 120 120 60 120H0Z" fill="white"/>
          </svg>
        </div>
      </section>

      {/* Por que escolher a Droop Race */}
      <section className="py-16 bg-gradient-to-b from-background to-muted">
        <div className="container mx-auto px-4">
          <h2 className="font-display text-3xl font-bold text-center mb-12">
            Por que escolher a <span className="text-primary">Droop Race</span>?
          </h2>
          <div className="grid grid-cols-2 lg:grid-cols-4 gap-6">
            {WHY_CHOOSE_US.map((item, index) => (
              <div 
                key={item.title}
                className="text-center group hover:transform hover:scale-105 transition-all duration-300"
                style={{ animationDelay: `${index * 100}ms` }}
              >
                <div className="inline-flex items-center justify-center w-20 h-20 rounded-full bg-primary/10 group-hover:bg-primary/20 transition-colors mb-4">
                  <item.icon className="h-10 w-10 text-primary" />
                </div>
                <h3 className="font-semibold text-lg mb-2">{item.title}</h3>
                <p className="text-sm text-muted-foreground">{item.desc}</p>
              </div>
            ))}
          </div>
        </div>
      </section>

      {/* Benefits */}
      <section className="border-y border-border bg-card">
        <div className="container mx-auto px-4 py-8">
          <div className="grid grid-cols-2 md:grid-cols-4 gap-4">
            {benefits.map((b, index) => (
              <div 
                key={b.title} 
                className="flex items-center gap-3 p-3 hover:bg-accent/5 rounded-lg transition-all group"
                style={{ animation: `fadeInUp 0.5s ease ${index * 0.1}s` }}
              >
                <div className="shrink-0 w-12 h-12 rounded-full bg-primary/10 flex items-center justify-center group-hover:scale-110 group-hover:bg-primary/20 transition-all">
                  <b.icon className="h-6 w-6 text-primary" />
                </div>
                <div>
                  <p className="font-semibold text-card-foreground">{b.title}</p>
                  <p className="text-sm text-muted-foreground">{b.desc}</p>
                </div>
              </div>
            ))}
          </div>
        </div>
      </section>

      {/* Categories */}
      <section className="container mx-auto px-4 py-16">
        <h2 className="font-display text-3xl font-bold text-foreground mb-8 text-center md:text-left">
          Nossas <span className="text-primary">Categorias</span>
        </h2>
        <div className="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-5 gap-4">
          {categories.map((cat, index) => (
            <Link
              key={cat.id}
              to={`/catalogo?cat=${cat.id}`}
              className="group bg-card border border-border rounded-xl p-6 text-center hover:border-primary hover:shadow-lg hover:-translate-y-1 transition-all duration-300"
              style={{ animationDelay: `${index * 50}ms` }}
            >
              <span className="text-5xl mb-3 block group-hover:scale-110 group-hover:rotate-3 transition-transform">
                {cat.icon}
              </span>
              <p className="font-medium text-card-foreground group-hover:text-primary transition-colors">
                {cat.name}
              </p>
              <span className="text-xs text-muted-foreground mt-1 block">
                {cat.count || 50}+ produtos
              </span>
            </Link>
          ))}
        </div>
      </section>

      {/* Featured Products */}
      <section className="bg-muted py-16">
        <div className="container mx-auto px-4">
          <div className="flex items-center justify-between mb-8">
            <div>
              <h2 className="font-display text-3xl font-bold text-foreground">
                Produtos em <span className="text-primary">Destaque</span>
              </h2>
              <p className="text-muted-foreground mt-2">Os mais vendidos da semana</p>
            </div>
            <Link to="/catalogo">
              <Button variant="ghost" className="text-primary group">
                Ver todos 
                <ArrowRight className="h-4 w-4 ml-1 group-hover:translate-x-1 transition-transform" />
              </Button>
            </Link>
          </div>
          <div className="grid grid-cols-2 lg:grid-cols-4 gap-4">
            {featuredProducts.map(p => (
              <ProductCardMemo key={p.id} product={p} />
            ))}
          </div>
        </div>
      </section>

      {/* New Products */}
      {newProducts.length > 0 && (
        <section className="container mx-auto px-4 py-16">
          <div className="flex items-center justify-between mb-8">
            <div>
              <h2 className="font-display text-3xl font-bold text-foreground">
                <span className="text-primary">Novidades</span> que Chegaram
              </h2>
              <p className="text-muted-foreground mt-2">Confira as últimas peças do estoque</p>
            </div>
            <Link to="/catalogo?sort=newest">
              <Button variant="ghost" className="text-primary group">
                Ver novidades
                <ArrowRight className="h-4 w-4 ml-1 group-hover:translate-x-1 transition-transform" />
              </Button>
            </Link>
          </div>
          <div className="grid grid-cols-2 lg:grid-cols-4 gap-4">
            {newProducts.map(p => (
              <ProductCardMemo key={p.id} product={p} />
            ))}
          </div>
        </section>
      )}

      {/* Offers */}
      {saleProducts.length > 0 && (
        <section className="bg-gradient-to-r from-accent/10 via-accent/5 to-transparent py-16">
          <div className="container mx-auto px-4">
            <div className="flex items-center justify-between mb-8">
              <div>
                <div className="inline-flex items-center gap-2 bg-accent/20 text-accent px-3 py-1 rounded-full text-sm font-medium mb-3">
                  <span className="relative flex h-2 w-2">
                    <span className="animate-ping absolute inline-flex h-full w-full rounded-full bg-accent opacity-75"></span>
                    <span className="relative inline-flex rounded-full h-2 w-2 bg-accent"></span>
                  </span>
                  OFERTAS IMPERDÍVEIS
                </div>
                <h2 className="font-display text-3xl font-bold text-foreground">
                  Promoções <span className="text-accent">Especiais</span>
                </h2>
                <p className="text-muted-foreground mt-2">Preços baixos por tempo limitado</p>
              </div>
              <Link to="/catalogo?discount=true">
                <Button variant="ghost" className="text-accent group">
                  Ver todas ofertas
                  <ArrowRight className="h-4 w-4 ml-1 group-hover:translate-x-1 transition-transform" />
                </Button>
              </Link>
            </div>
            <div className="grid grid-cols-2 lg:grid-cols-4 gap-4">
              {saleProducts.slice(0, 4).map(p => (
                <ProductCardMemo key={p.id} product={p} />
              ))}
            </div>
          </div>
        </section>
      )}

      {/* Newsletter */}
      <NewsletterSection />

      {/* WhatsApp Button */}
      <WhatsAppButton />

      {/* Bottom Navigation (mobile) */}
      <BottomNav />

      {/* CSS para animações */}
      <style jsx>{`
        @keyframes fadeInUp {
          from {
            opacity: 0;
            transform: translateY(20px);
          }
          to {
            opacity: 1;
            transform: translateY(0);
          }
        }
      `}</style>
    </>
  );
}
