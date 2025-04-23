# Segredo-Digital
Banner.tsx
export default function Banner() {
  return (
    <div className="bg-[#ff4d4f] py-3 px-4 text-white text-center font-bold text-sm sm:text-base">
      <div className="container mx-auto">
        VAGAS LIMITADAS — GARANTIA DE 7 DIAS + OFERTA ESPECIAL DE R$19,90 POR TEMPO LIMITADO!
      </div>
    </div>
  );
}
Hero.tsx
import { PAYMENT_LINK } from "@/lib/constants";
export default function Hero() {
  return (
    <header 
      className="relative flex items-center justify-center min-h-screen bg-cover bg-center" 
      style={{backgroundImage: url('https://images.unsplash.com/photo-1611162616475-46b635cb6868?auto=format&fit=crop&w=1470&q=80')}}
    >
      <div className="absolute inset-0 bg-gradient-to-b from-black/75 to-black/85"></div>
      <div className="container mx-auto z-10 px-4">
        <div className="max-w-3xl mx-auto text-center bg-[#0d1117]/80 backdrop-blur-sm p-8 sm:p-10 rounded-xl shadow-xl">
          <h1 className="text-4xl sm:text-5xl md:text-6xl font-extrabold mb-4 text-[#00ff9f] leading-tight">
            Ganhe Dinheiro com o TikTok em 7 Dias
          </h1>
          <p className="text-lg sm:text-xl mb-8 text-[#e6edf3]">
            Aprenda a criar contas dark, monetizar sem aparecer e faturar até R$500/semana começando do zero
          </p>
          <a 
            href={PAYMENT_LINK}
            className="inline-block bg-[#00ff9f] hover:bg-[#02d383] text-[#0d1117] font-bold py-4 px-8 rounded-full text-lg transition-all duration-300 transform hover:scale-105 shadow-lg"
          >
            Quero Começar Agora
          </a>
        </div>
      </div>
    </header>
  );
}
CountdownSection.tsx
import { useCountdown } from "@/lib/hooks";
import { Clock, Check, RotateCw } from "lucide-react";
export default function CountdownSection() {
  const { hours, minutes, seconds, isExpired } = useCountdown(24 * 60 * 60); // 24 hours in seconds
  
  return (
    <section className="bg-[#0d1117] py-12 sm:py-16">
      <div className="container mx-auto px-4 text-center">
        <h2 className="text-2xl sm:text-3xl font-bold text-[#ff4d4f] mb-4">
          {isExpired ? "Oferta Expirada!" : "Oferta de R$19,90 expira em:"}
        </h2>
        <div className="text-3xl sm:text-4xl font-bold text-[#00ff9f]">
          {isExpired ? "Tempo Esgotado!" : ${hours.toString().padStart(2, '0')}h ${minutes.toString().padStart(2, '0')}m ${seconds.toString().padStart(2, '0')}s}
        </div>
        <div className="mt-8 flex flex-col sm:flex-row justify-center items-center gap-6">
          <div className="flex items-center gap-2">
            <Clock className="h-6 w-6 text-[#00ff9f]" />
            <span>Acesso vitalício</span>
          </div>
          <div className="flex items-center gap-2">
            <RotateCw className="h-6 w-6 text-[#00ff9f]" />
            <span>Começe imediatamente</span>
          </div>
          <div className="flex items-center gap-2">
            <Check className="h-6 w-6 text-[#00ff9f]" />
            <span>Garantia de 7 dias</span>
          </div>
        </div>
      </div>
    </section>
  );
}
The hooks.ts file (for countdown functionality)
import { useState, useEffect } from "react";
export function useCountdown(initialSeconds: number) {
  const [timeLeft, setTimeLeft] = useState(initialSeconds);
  const [isExpired, setIsExpired] = useState(false);
  useEffect(() => {
    // Try to get the saved countdown timestamp from localStorage
    const savedEndTime = localStorage.getItem('countdownEndTime');
    
    if (savedEndTime) {
      // If we have a saved end time, calculate how much time is left
      const endTime = parseInt(savedEndTime, 10);
      const now = Math.floor(Date.now() / 1000);
      const remaining = endTime - now;
      
      if (remaining > 0) {
        setTimeLeft(remaining);
      } else {
        setIsExpired(true);
        setTimeLeft(0);
      }
    } else {
      // If no saved end time, set a new one
      const endTime = Math.floor(Date.now() / 1000) + initialSeconds;
      localStorage.setItem('countdownEndTime', endTime.toString());
    }
    
    const timer = setInterval(() => {
      setTimeLeft((prevTime) => {
        if (prevTime <= 1) {
          clearInterval(timer);
          setIsExpired(true);
          return 0;
        }
        return prevTime - 1;
      });
    }, 1000);
    return () => clearInterval(timer);
  }, [initialSeconds]);
  const hours = Math.floor(timeLeft / 3600);
  const minutes = Math.floor((timeLeft % 3600) / 60);
  const seconds = timeLeft % 60;
  return { hours, minutes, seconds, isExpired };
}
BenefitsSection.tsx
import { BarChart3, Zap, DollarSign, RefreshCw } from "lucide-react";
const benefits = [
  {
    title: "Contas TikTok Dark",
    description: "Aprenda a criar contas que viralizam sem mostrar seu rosto ou revelar sua identidade",
    icon: <BarChart3 className="h-6 w-6" />
  },
  {
    title: "Estratégias de Conteúdo",
    description: "Descubra as fórmulas de conteúdo que geram cliques e comissões garantidas",
    icon: <Zap className="h-6 w-6" />
  },
  {
    title: "Monetização Imediata",
    description: "Passo a passo completo para transformar visualizações em renda passiva",
    icon: <DollarSign className="h-6 w-6" />
  },
  {
    title: "Automação e Escala",
    description: "Como escalar de R$0 a R$500 semanais e automatizar todo o processo",
    icon: <RefreshCw className="h-6 w-6" />
  }
];
export default function BenefitsSection() {
  return (
    <section className="bg-[#161b22] py-16 sm:py-20">
      <div className="container mx-auto px-4">
        <h2 className="text-3xl sm:text-4xl font-bold text-center text-[#00ff9f] mb-12">O Que Você Vai Aprender:</h2>
        <div className="grid md:grid-cols-2 gap-8 max-w-4xl mx-auto">
          {benefits.map((benefit, index) => (
            <div key={index} className="bg-[#0d1117] p-6 rounded-lg shadow-lg hover:shadow-xl transition-shadow">
              <div className="flex items-start">
                <div className="mr-4 bg-[#00ff9f]/20 rounded-full p-3 text-[#00ff9f]">
                  {benefit.icon}
                </div>
                <div>
                  <h3 className="text-xl font-semibold mb-2">{benefit.title}</h3>
                  <p className="text-[#e6edf3]/90">{benefit.description}</p>
                </div>
              </div>
            </div>
          ))}
        </div>
      </div>
    </section>
  );
}
PricingSection.tsx
import { Check } from "lucide-react";
import { PAYMENT_LINK } from "@/lib/constants";
const features = [
  "Curso completo \"Ganhe Dinheiro com o TikTok em 7 Dias\"",
  "Lista de Nichos Lucrativos (Bônus)",
  "Checklist de Conteúdo Viral (Bônus)",
  "Acesso vitalício e atualizações gratuitas",
  "Garantia de 7 dias ou seu dinheiro de volta"
];
export default function PricingSection() {
  return (
    <section className="bg-[#0d1117] py-16">
      <div className="container mx-auto px-4 text-center">
        <h2 className="text-4xl sm:text-5xl font-bold mb-4 text-[#00ff9f]">
          Preço Promocional: <span className="text-[#ff4d4f] line-through mr-2">R$97,00</span> R$19,90
        </h2>
        <p className="text-xl max-w-3xl mx-auto mb-8">
          Acesso imediato ao conteúdo + bônus exclusivos. Investimento único, sem mensalidades.
        </p>
        
        <div className="max-w-xl mx-auto bg-[#161b22] p-8 rounded-xl shadow-lg border border-gray-800 mb-8">
          <h3 className="text-2xl font-bold mb-6">O Que Está Incluído:</h3>
          <ul className="space-y-4 text-left mb-8">
            {features.map((feature, index) => (
              <li key={index} className="flex items-start">
                <Check className="h-6 w-6 text-[#00ff9f] mr-2 flex-shrink-0 mt-0.5" />
                <span dangerouslySetInnerHTML={{ __html: 
                  feature.includes("(Bônus)") 
                    ? feature.replace("(Bônus)", "<span class='text-[#ff4d4f] font-bold'>(Bônus)</span>") 
                    : feature 
                }} />
              </li>
            ))}
          </ul>
          
          <a 
            href={PAYMENT_LINK} 
            className="block w-full bg-[#00ff9f] hover:bg-[#02d383] text-[#0d1117] font-bold py-4 px-6 rounded-full text-lg transition-all duration-300 transform hover:scale-105 shadow-lg"
          >
            Comprar Agora
          </a>
          
          <p className="mt-4 text-sm text-gray-400">Pagamento seguro via Cartão, Pix ou Boleto</p>
        </div>
        
        <div className="text-center">
          <p className="text-xl font-bold text-[#ff4d4f]">Atenção: Oferta por tempo limitado!</p>
          <p className="text-lg">O preço aumentará quando o contador chegar a zero.</p>
        </div>
      </div>
    </section>
  );
}
constants.ts
export const PAYMENT_LINK = "https://pay.kirvano.com/b5ca734d-6db1-4a17-bdfb-e8bd6ea9c641";
Home.tsx (Main Page)
import Banner from "@/components/Banner";
import Hero from "@/components/Hero";
import CountdownSection from "@/components/CountdownSection";
import BenefitsSection from "@/components/BenefitsSection";
import BonusSection from "@/components/BonusSection";
import GuaranteeSection from "@/components/GuaranteeSection";
import TestimonialsSection from "@/components/TestimonialsSection";
import PricingSection from "@/components/PricingSection";
import Footer from "@/components/Footer";
export default function Home() {
  return (
    <div className="font-sans bg-[#0d1117] text-[#e6edf3] antialiased min-h-screen">
      <Banner />
      <Hero />
      <CountdownSection />
      <BenefitsSection />
      <BonusSection />
      <GuaranteeSection />
      <TestimonialsSection />
      <PricingSection />
      <Footer />
    </div>
  );
}
